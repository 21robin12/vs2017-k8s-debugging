// BUILD IMAGE & RUN CONTAINER
// ---------------------------
dotnet publish -c Debug
docker build -t docker.io/21robin12/debugging:0.1 .
docker push docker.io/21robin12/debugging:0.1
kubectl apply -f kubernetes.yaml
kubectl port-forward debugging-pod 1337:80
// now visit http://localhost:1337/api/values - should see api response

// ATTACH DEBUGGER
// ---------------
// from Visual Studio Command Window, run this (note the absolute path to attach.xml, and that this file contains another absolute path - not sure how to fix this)
Debug.MIDebugLaunch /Executable:dotnet /OptionsFile:C:\_REPOS\DebuggingMIEngine\DebuggingMIEngine\attach.xml

// Things to improve...
// --------------------

Include installing debugger in Dockerfile (to save time on debugger startup)? How to do this only on dev images and not any that will go to production?