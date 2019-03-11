---
title: Introduction to Anuglar NX
date: 2019-03-07 20:24:28
tags: work
category: knowledge
---

# Why Nx?
- an extention of the Angular CLI
- allow multiple Angular applications in one app (easily share code among multiple applications)
- encourage code sharing
- encourage to architect your app properly

Multiple repos vs monorepo
- enterprise large apps
- consistency
- formalize a community approach
- robust and proven patterns

Steps:

1. install Angular CLI
   ``` batch
   npm install -g @angular/cli
   ```
2. install Nx
   ``` batch
   npm install -g @nrwl/schematics
   ```
3. create new Nx workspace
   ``` batch
   create-nx-workspace <name>
   ```
4. Now you have a new workspace
   1. apps (where the application lives)
   2. libs (reusable chunks of functionality)
   3. tools (things to manage your workspace)
5. 
6. 