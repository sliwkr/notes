# OWASP ZAP

## table of contents

- [Window not rendered on tiling window manager](#wm-render)
- [Import ZAP Root CA into the browser](#zap-ca-import)
- [Topic](#header-link)

## snippets

### Header-link-template

```lang

```

### WM render

Allegedly common among java apps on DWM.

https://www.reddit.com/r/suckless/comments/girs3z/dwm_and_reparenting/

```sh
# ~/.xinitrc
export _JAVA_AWT_WM_NONREPARENTING=1
```


### ZAP CA import

The certificate is created automatically when running ZAP for the first time.

#### Get certificate

GUI: Zap options -> Dynamic SSL Certificate

CLI:

```sh
zap.sh -certfulldump /absolute/path/zap-root-ca.pem
# log exerpt:
# [ZAP-BootstrapGUI] INFO  org.parosproxy.paros.CommandLine - Root CA certificate written to /home/kks/zap-root-ca.pem
```

#### Import certificate into the browser

##### Firefox

Options > Certificates > View Certificates > Authorities > Import > select zap-root-ca.pem

##### Chrome

Settings > Privacy and security > Manage certificates > Authorities > Import > select zap-root-ca.pem > Trust this certificate for identifying websites

