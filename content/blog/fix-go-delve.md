+++
title = 'Fix M1 Mac Go Delve Issues'
date = 2024-05-18T15:48:26-05:00
+++

# Introduction
I recently upgraded to the lastest Go version on my M1 Mac and suddenly was unable to debug my code in VSCode.  Using the debugger resulted in the error `error:could not launch process: can not run under Rosetta, check that the installed build of Go is right for your CPU architecture`.  Let's dive into this issue and figure out how to fix it.

# Explanation
The error message say that the Go build is for the wrong architecture. We can check that by running `echo $GOARCH` on command line. What you expect to see is ARM64 as the correct architecture for an M1 Mac, however I had mistakenly grabbed the AMD64 architecture since it's the first one listed for Mac on Go's website currently.  

# Fix
First, we need to download and install the correct architecture.  You can do this using Go's website and then running `echo $GOARCH` again to verify.  However, this doesn't fix the entire problem.  Next you'll want to uninstall the Go extension in VSCode.  Lastly, you'll want to run the following commands from command line to ensure you have the proper debug tools for your architecture.
```bash
go install github.com/go-delve/delve/cmd/dlv@latest
go install github.com/aarzilli/gdlv@latest
```
Finally, install Go tools in VSCode again and start debugging!