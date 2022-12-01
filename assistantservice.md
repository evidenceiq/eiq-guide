# Packages path

.../BUILD/SERVICE/PARTS/AssistantService/AssistantService.2.0.3

# Installation

## For windows

Run 
```sh
assistantservice.exe
```


When console ask

```sh
Enter service name [Enter without type for default name (AssistantService)]: type new name your want to change and press ENTER
```

```sh
Do you want to create install and uninstall service files [yes / no or press Enter]?: yes
```

```sh
Do you want to use custom authentication for this service [yes / no or press Enter]?: yes
```
```sh
Enter service username [Enter without type for default account]: type username for service, default is user running this EXE.
```
```sh
Enter service password [Enter without type for empty password]: type the password for account above.
```
<b>Install service</b>

```cmd
install-service.bat
```
<b>Start service</b> 

```sh
start-service.bat
```

<b>Check service</b>  

Go to url [http://localhost:4051](http://localhost:4051)

## For ubuntu:

<b>Assign file access</b> 

```sh
sudo chmod 777 ./assistantservice
```

Run <b>assistantservice</b>

```sh
sudo ./assistantservice
```
When console ask

```sh
Enter service name [Enter without type for default name (AssistantService)]: type new name your want to change and press ENTER
```

```sh
Do you want to create install and uninstall service files [yes / no or press Enter]?: yes
```

<b>Install service</b> 
```sh
sudo ./install-service 
```

<b>Start service</b> 
```sh
sudo ./start-service 
```

<b>Check service</b>  
Go to url [http://localhost:4051](http://localhost:4051)