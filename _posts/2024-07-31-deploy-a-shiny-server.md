---
layout: single
title:  "Deploy a Shiny server on digital ocean Ubuntu droplet"
date:   2024-07-31 18:45:00 +1000
tags: shiny ubuntu  
excerpt: "This post is set out to explain in a step by step manner how to deploy shinny apps on your own server using Ubuntu. "
---

![Shiny logo]({{ site.url }}{{ site.baseurl }}/assets/images/shiny_logo.jpg)

Shiny is a powerful web application framework for python and R, enabling the creation of interactive data visualizations deployed as web apps. To run these applications on a server, you need to install Shiny Server. In this blog post, we will walk you through the process of installing Shiny Server on Ubuntu.

## Prerequisites:
Before we begin, make sure you have the following :

1. An Ubuntu server with root access.
2. Basic knowledge of working with the Linux command line.

If you don't have a Ubuntu droplet already, follow this [link](https://m.do.co/c/74f204b4fdd4) to set one up. You will need to set up an account to create the droplet. I recommend the a low cost option. You can always upgrade later. I'm using Ubuntu 22.04 LTS (x64) for this example. You can see the instructions for setting up a droplet [here](https://docs.digitalocean.com/products/droplets/how-to/create/). There are [clear instructions here](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-22-04) for the inital server setup using a non-root user with elevated privileges.


### Update everything on the server
Now that you have logged on to your droplet, let's ensure that all system packages are up to date by running the following commands:

```
sudo apt update
sudo apt upgrade
```

### Install R and the packages
To run Shiny Server, we need to have R installed on our system. This can be a pain to set up securely. Luckily there is a helpful guide on on [CRAN](https://cran.rstudio.com/bin/linux/ubuntu/) to atke the pain out of it. I have included the steps for ceonvenience.

``` bash
# update indices
sudo apt update -qq

# install two helper packages we need
sudo apt install --no-install-recommends software-properties-common dirmngr

# add the signing key (by Michael Rutter) for these repos
# To verify key, run gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc 
# Fingerprint: E298A3A825C0D65DFD57CBB651716619E084DAB9
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc

# add the R 4.0 repo from CRAN -- adjust 'focal' to 'groovy' or 'bionic' as needed
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
```

```
sudo apt install --no-install-recommends r-base
```

### Install Required R Packages
Now that we have R installed, let's install some necessary packages. There are many ways to do this. I like to make things easy on myself and  get them from a repo. This is a good way to ensure that you have the latest versions and it's easy to update them later.

``` bash
# First add the repo key ID.
sudo add-apt-repository ppa:c2d4u.team/c2d4u4.0+

# Then install the packages (after updating the package index)
apt install --no-install-recommends r-cran-tidyverse r-cran-shiny r-cran-rmarkdown

# You can keeping adding to the list as needed if you intend to use R for other things.
```

### Download and Install Shiny Server
To download and install Shiny Server, execute the following commands:

```
wget https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.16.958-amd64.deb
sudo dpkg -i shiny-server-1.5.16.958-amd64.deb
```

Note: Make sure to replace `shiny-server-1.5.16.958-amd64.deb` with the latest version available from https://www.rstudio.com/products/shiny/download-server/.

Step 5: Start and Verify Shiny Server
To start Shiny Server, use the following command:

```
sudo systemctl start shiny-server
```

To verify if Shiny Server is running correctly, open a web browser and enter your server's IP address or domain name followed by `:3838`. You should see the default Shiny Server landing page.

Step 6: Configure Shiny Server (Optional)
By default, Shiny Server looks for applications in the `/srv/shiny-server/` directory. If you want to host your own applications, place them in this directory. Additionally, you can customize the configuration file located at `/etc/shiny-server/shiny-server.conf` to modify various settings like port number, log location, etc.

Step 7: Managing Shiny Server
To manage Shiny Server as a service, use the following commands:

- Start: `sudo systemctl start shiny-server`
- Stop: `sudo systemctl stop shiny-server`
- Restart: `sudo systemctl restart shiny-server`

Conclusion:
Congratulations! You have successfully installed and set up Shiny Server on your Ubuntu machine. Now you can begin developing and deploying interactive web applications using R's powerful visualization capabilities. Explore further documentation on https://shiny.rstudio.com/ to unlock the full potential of Shiny!

Remember to keep your system updated with security patches and regularly back up your applications for a smooth and secure experience with Shiny Server. Happy coding!