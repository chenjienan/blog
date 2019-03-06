---
title: JSON Web Token
date: 2019-03-05 11:37:44
tags:
category: knowledge
---
# Explain JWT like I'm five
### Scenario 1: (Not JWT)

I'm at a restaurant and I order a beer. This particular restaurant takes age verification very seriously. So the waiter asks to see my license. I provide it, but then he goes and gets a phone. He calls the DMV, waits on hold for a bit, and asks them to verify the license details. Once he talks to the licensing authority and they confirm my date of birth, then he goes and gets my beer.

This is the old style of authentication, where you present your session cookie. When the server receives the session ID from the cookie, it turns around and calls the session service (or queries a database or memory) to find out if your ID is still good, and additional information that might be stored in that session.

As you can see, this can become a bottleneck to service. And the restaurant probably won't stay in business long.

### Scenario 2: (JWT)

I'm at a restaurant and I order a beer. This particular restaurant also takes age verification very seriously. The waiter asks to see my license, and I provide it. The waiter pulls out a UV light and inspects the watermark on my license. It checks out ok, so he hands the license back and gets my beer.

In this case, the issuing authority of the license placed a special "seal" into the license that can be used to identify valid licenses. This means that verification can be performed without calling back to the DMV. The waiter has to know exactly what seal to look for. That might mean he has to go look up the state's seal sometimes. Once the waiter determines that the license is valid, they can trust the Date of Birth information on it.

This is the JWT variety of authentication. Once the DMV believes you are who you say (JWT version: autheticated, probably with password), it collects various data about you (JWT version: claims) and puts it on the license (JWT itself). When the license is issued, it is also watermarked with a seal (JWT version: digital signature) so that it can be examined for validity by people who know what to look for (JWT version: your API validates the JWT signature with a shared key). After that, the license (JWT) is trusted and the Date of Birth (claim) is assumed to be true.

- [Home page](https://jwt.io/)
- [Intro to JWT](https://jwt.io/introduction/)
- [NuGet package for .NET](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/)
- [Angular package](https://github.com/auth0/angular2-jwt)

# backend C#　implementation:


### Static jwt manager
``` C#
// Static jwt manager
// 3 methods
public static class JwtManager
    {
        private static readonly HMACSHA256 Secret = new HMACSHA256();

        public static string GenerateToken(string claimA, string claimB, int expireMinutes = 30)
        {
            var symmetricKey = Secret.Key;
            var tokenHandler = new JwtSecurityTokenHandler();
            var now = DateTime.UtcNow;
            var tokenDescriptor = new SecurityTokenDescriptor
            {
                Subject = new ClaimsIdentity(new[]
                    {
                        new Claim("claimA", claimA),
                        new Claim("claimB", claimB) 
                    }),

                Lifetime = new Lifetime(now, now.AddMinutes(expireMinutes)),
                SigningCredentials = new SigningCredentials(
                    new InMemorySymmetricSecurityKey(symmetricKey),                    
                    SecurityAlgorithms.HmacSha256Signature,
                    SecurityAlgorithms.Sha256Digest)
            };

            var securityToken = tokenHandler.CreateToken(tokenDescriptor);
            var token = tokenHandler.WriteToken(securityToken);

            return token;
        }

        public static string RefreshToken(string token, int expireMinutes = 30)
        {
            var principal = GetPrincipal(token);
            var identity = principal?.Identity as ClaimsIdentity;

            var claimAId = identity?.FindFirst("claimA");
            var claimBId = identity?.FindFirst("claimB");
            var claimA = claimAId?.Value;
            var claimB = claimBId?.Value;

            if (string.IsNullOrEmpty(claimA) || string.IsNullOrEmpty(claimB))
            {
                throw new Exception("Either claimA or claimB is null or empty.");
            }
            return GenerateToken(claimA, claimB, expireMinutes);
        }

        public static ClaimsPrincipal GetPrincipal(string token)
        {
            try
            {
                var tokenHandler = new JwtSecurityTokenHandler();
                var jwt = tokenHandler.ReadToken(token) as JwtSecurityToken;

                if (jwt == null) return null;

                // Check token expiration
                if (DateTime.Compare(DateTime.UtcNow, jwt.ValidTo) > 0) return null;                

                var symmetricKey = Secret.Key;
                var validationParameters = new TokenValidationParameters()
                {
                    RequireExpirationTime = true,
                    ValidateIssuer = false,
                    ValidateAudience = false,
                    IssuerSigningKey = new InMemorySymmetricSecurityKey(symmetricKey)
                };

                var principal = tokenHandler.ValidateToken(token, validationParameters, out _);

                return principal;
            }

            catch (Exception)
            {
                return null;
            }
        }
    }

```

### MVC attribute (decorator)
``` C#
// add MVC attribute to guard the routes
public class JwtAuthenticationAttribute: Attribute, IAuthenticationFilter
    {
        public string Realm { get; set; }
        public bool AllowMultiple => false;

        public async Task AuthenticateAsync(HttpAuthenticationContext context, CancellationToken cancellationToken)
        {
            var request = context.Request;
            var authorization = request.Headers.Authorization;

            if (authorization == null || 
                string.IsNullOrEmpty(authorization.Parameter) ||
                !authorization.Scheme.Equals("bearer", StringComparison.OrdinalIgnoreCase))
            {
                context.ErrorResult = new AuthenticationFailureResult("Missing Jwt Token", request);
                return;
            }

            var token = authorization.Parameter;
            var principal = await AuthenticateJwtToken(token);

            if (principal == null)
                context.ErrorResult = new AuthenticationFailureResult("Invalid token", request);
            else
                context.Principal = principal;
        }


        protected Task<IPrincipal> AuthenticateJwtToken(string token)
        {
            if (ValidateToken(token, out var claimA, out var claimB))
            {
                var claims = new List<Claim>
                {
                    new Claim("claimA", claimA),
                    new Claim("claimB", claimB) 
                    // Add more claims if needed
                };
                var identity = new ClaimsIdentity(claims, "Jwt");
                IPrincipal user = new ClaimsPrincipal(identity);
                return Task.FromResult(user);                
            }
            return Task.FromResult<IPrincipal>(null);
        }

        private static bool ValidateToken(string token, out string claimA, out string claimB)
        {
            claimA = null;
            claimB = null;

            var simplePrinciple = JwtManager.GetPrincipal(token);
            var identity = simplePrinciple?.Identity as ClaimsIdentity;

            if (identity == null) return false;
            if (!identity.IsAuthenticated) return false;

            var claimAId = identity.FindFirst("claimA");
            var claimBId = identity.FindFirst("claimB");
            claimA = claimAId?.Value;
            claimB = claimBId?.Value;

            if (string.IsNullOrEmpty(claimA) || string.IsNullOrEmpty(claimB)) return false;            
            // Put more validation here if needed
            
            return true;
        }

        public Task ChallengeAsync(HttpAuthenticationChallengeContext context, CancellationToken cancellationToken)
        {
            Challenge(context);
            return Task.FromResult(0);
        }

        private void Challenge(HttpAuthenticationChallengeContext context)
        {
            string parameter = null;

            if (!string.IsNullOrEmpty(Realm))
            {
                parameter = "realm=\"" + Realm + "\"";
            }
            context.ChallengeWith("Bearer", parameter);
        }
    }
```
#### [stack overflow question](https://stackoverflow.com/questions/40281050/jwt-authentication-for-asp-net-web-api) (make sure you go thru the comments)


#### more resources:
- [JWT with angular](https://blog.angular-university.io/angular-jwt-authentication/)
- [Good explanation](https://blog.angular-university.io/angular-jwt/)