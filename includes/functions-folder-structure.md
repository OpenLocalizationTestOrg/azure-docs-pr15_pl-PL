
Kod wszystkich funkcji w aplikacji dla danej funkcji znajduje się w folderu głównego, który zawiera plik konfiguracji hosta i co najmniej jeden podfolder, z których każdy będzie zawierać kod oddzielnych funkcji, tak jak w poniższym przykładzie

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Plik *host.json* zawiera niektóre środowisko uruchomieniowe konfiguracyjne i znajduje się w folderze głównym aplikacji funkcji. Aby uzyskać informacje na temat ustawień, które są dostępne zobacz [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) na stronie typu wiki z repozytorium WebJobs.Script.

Każda funkcja ma folder, który zawiera jeden lub więcej plików kodu, konfiguracji function.json i inne zależności.