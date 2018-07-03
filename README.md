# vs2017-k8s-debugging

Mostly from this article: https://medium.com/@pavel.agarkov/debugging-asp-net-core-app-running-in-kubernetes-minikube-from-visual-studio-2017-on-windows-6671ddc23d93

## Testing this example

### Building & pushing the image

*Already done, see https://hub.docker.com/r/21robin12/debugging/*

```
dotnet publish -c Debug
docker build -t docker.io/21robin12/debugging:0.1 .
docker push docker.io/21robin12/debugging:0.1
```

### Deploying to k8s cluster

```
kubectl apply -f kubernetes.yaml
kubectl port-forward debugging-pod 1337:80
```

Now visit http://localhost:1337/api/values - should see an API response

### Attaching the debugger

 - Update the absolute path to attach.xml in the following command
 - Update the absolute path to attach.ps1 in attach.xml
 - Run the following command from VS2017 Command Window
 
```
Debug.MIDebugLaunch /Executable:dotnet /OptionsFile:C:\_REPOS\DebuggingMIEngine\DebuggingMIEngine\attach.xml
```

## Things to improve

Include installing debugger in Dockerfile (to save time on debugger startup)? How to do this only on dev images and not any that will go to production?