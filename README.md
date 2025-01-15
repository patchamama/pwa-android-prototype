This repository is my guide to remember the tips I have learned that allow me to create as simple as possible android applications using pwabuilder.com. 

## Execute locally the application to be displayed on the pwa and display it in the android simulator

The first thing is to have a development environment that allows you to quickly work with the application locally and at the same time to see the changes in the android simulator to adjust what we want to obtain. To do this we must take into account:

- pwabuilder.com creates by default the application to a site using ssl (https). 
- You can access the host operating system using the address 10.0.0.2 from the android simulator (if you have a page running for example in react on the host at the url: http://localhost:3000, you can access it from chrome in the android simulator by typing in the url: http://10.0.0.2:3000). 

To access directly from the simulator to a page that we are running on the host operating system, but that is accessed using ssl, we will create a reverse proxy with the help of nginx (as a sample I will configure everything with macOSx but in gnu/linux it is similar). 

- Install nginx:

```sh
brew install nginx
```

To find out where the configuration file is located we can run: `nginx -t`. Knowing the path we can add the following configuration to redirect any https from localhost to http://localhost:3000 -> `sudo nano /path-to-conf-file/nginx.conf`:

```
http {
    # Configuración HTTPS para el puerto 443 y redirección al puerto 3000
    servidor {
        listen 443 ssl;
        nombre_servidor localhost;

        ssl_certificate /Users/test/workspace/nginx/localhost.pem;
        ssl_certificate_key /Users/test/workspace/nginx/localhost-key.pem;

        ubicación / {
            proxy_pass http://localhost:3000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $dirección_remota;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
  }
```

Guardar y ejecutar nginx:

```sh
sudo nginx -s reload
```

