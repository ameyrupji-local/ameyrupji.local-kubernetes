# ameyrupji.local-kubernetes-setup

This repository outlines the steps to setup nginx on MacOS to act as a reverse proxy for various apps deployed on my local server.

## Prerequisites 

- HomeBrew installed (Installation instructions: https://www.howtogeek.com/211541/homebrew-for-os-x-easily-installs-desktop-apps-and-terminal-utilities/)

## System Configuration at time of test

- macOS Mojave - Version 10.14.5
- Docker Installed

## Installation instructions

### Install Docker on MacOS

Download and install docker for MacOS from `hub.docker.com`. You will need to authenticate to be able to download the app.

![docker app download](images/docker-app-download.png)

Install the downloaded app and start it. It will appear in the menu bar.

![docker app install](images/docker-app-install.png)

![docker app starting](images/docker-app-starting.png)

Wait for the app to start once the app is started to go the `Preferences` and click on the `Kubernetes` tab and check the `Enable Kubernetes` and `Show system containers (advanced)` and click on `Apply` button.

![docker app install kubernetes](images/docker-app-enable-kubernetes.png)

Wait for the Kubernetes to install it should take about 5-10 min.

![docker app install kubernetes](images/docker-app-kubernetes-installing.png)

### Deploy official Kubernetes dashboard

We can deploy the dashboard with the following command:

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml`

This should create all the necessary objects for the UI to run properly, which you can check by running `kubectl proxy`

![terminal install kubernetes dashboard](images/terminal-install-kubernetes-dashboard.png)

To Verify dashboard service is running run the following command in terminal: `kubectl get services -n kube-system`

![terminal verify dashboard is running](images/terminal-verify-dashboard-is-running.png)

Open `http://127.0.0.1:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/` in the browser to load the dashboard.

![safari kubernetes dashboard login](images/safari-kubernetes-dashboard-login.png)

Since this is deployed to our private cluster, we need to access it via a proxy. Kube-proxy is available to proxy our requests to the dashboard service. In your workspace, run the following command:

`kubectl proxy --port=8080 --address='0.0.0.0' --disable-filter=true &`

This will start the proxy, listen on port 8080, listen on all interfaces, and will disable the filtering of non-localhost requests.


### 


<!-- ## Test 

Open Safari it by going to URL:
`http://localhost`

![safari nginx port 80](images/safari-nginx-80.png)

This website should also be accessible over the network from another computer at `http://ameyrupji.local/`

![network safari nginx](images/network-safari-nginx.png)



## Cleanup

To uninstall nginx:

First stop the nginx server if it is running by:
`sudo nginx -s stop`

Using brew to uninstall Nginx run the command: 
`brew uninstall nginx`

Remove Nginx code by running the following commands:
`rm -f -R /usr/local/nginx` and `rm -f /usr/local/sbin/nginx` -->


## Useful Links

- https://medium.com/containermind/a-beginners-guide-to-kubernetes-7e8ca56420b6
- https://8gwifi.org/docs/kube-dash.jsp 
- https://stackoverflow.com/questions/46664104/how-to-sign-in-kubernetes-dashboard
- https://eksworkshop.com


# TODOs

- https://www.linode.com/docs/applications/containers/how-to-deploy-nginx-on-a-kubernetes-cluster/
- https://helm.sh/docs/chart_template_guide/#the-chart-template-developer-s-guide