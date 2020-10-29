# Uchicago Midway RCC Tutorial

This is a tutorial for using Uchicago Midway research computing center (RCC). 

*This tutorial was adapted from [RCC Website](https://rcc.uchicago.edu/docs/connecting/index.html). I highly recommend you to check out the website for detailed questions.*

## Connecting to RCC

### Connecting with SSH

SSH provides command-line access and interaction with the Midway2 computing cluster. For Linux and MacOS user, open the terminal and type in the command line

```
ssh <CNetID>@midway2.rcc.uchicago.edu
```

Please repalce `<CNetID>` with your own CNetID at Uchicago. Then enter the password associated to your ID when prompted for a password. A Duo two-factor autentication window will then pop up requesting you select from the available 2FA options to authenticate to midway2.

![](https://rcc.uchicago.edu/docs/_images/duo-2fa.png)

You can also enable X11 forwarding to run graphical user interface (GUI) from the server on your local machine. To do that, make sure to install [XQuartz](https://www.xquartz.org/) and include the `-Y` flag in the command

```
ssh -Y <CNetID>@midway2.rcc.uchicago.edu
```

### Connecting with ThinLinc

Alternatively ThinLinc is a remote desktop server that can be used to connect to RCC systems and obtain a remote GUI. RCC recommands that ThinLinc be used when running software that requires a GUI. To use ThinLinc to connect to Midway2, one can either use the web browser version or download and install the ThinLinc Desktop Client. The web browser version does not require any setup, but the performance will be less optimal than the Desktop Client version.

#### ThinLinc Web browser

To use ThinLinc in web browser, point to the [address](https://midway2.rcc.uchicago.edu). Log in with your CNetID and password, and complete the 2FA as in last subsection.

#### ThinLinc Deskop Client

Download and install the proper ThinLinc client from [ThinLinc download page](https://www.cendio.com/thinlinc/download).

Open the client and use the following information to log in

![](https://rcc.uchicago.edu/docs/_images/thinlinc-client.png)

The default setting for ThinLinc is for the client to open in a fullscreen window that fills “all monitors”. This may cause problems if you want to switch to other windows on your local machine. If you would prefer to work with ThinLinc from its own window, click Options from the initial login interface and then Screen to adjust your settings as desired. The following is an example of a setup that places the ThinLinc client in its own window:

<img src="https://rcc.uchicago.edu/docs/_images/thinlinc-options.png" width="600"/>
