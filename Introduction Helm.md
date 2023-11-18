## Helm ??

## Evolution 
```
APP → Dockerfile → Docker Image → Repo (registry)
App → Docker-Compose
YAML → Kubernetes
Package YAML → Helm
```

### → Package Manager for Kubernete's
```
LINUX - RHEL   = Install/Update/remove (Packages) = YUM (Client) → REPO contains PACKAGE
LINUX - UBUNTU = Install/Update/remove (Packages) = APT (Client) → REPO contains PACKAGE
WINDOWS = Install/Update/Remove (Programs) = Chocolatey (Client) → REPO contains PACKAGE
MAC OS  = Install/Update/remove (Applications) = Homebrew (Client) → REPO contains PACKAGE
MOBILE = Install/Update/remove (Android) = Playstore (Client) → REPO contains PACKAGE
```
```
PLATFORM  → Client → REPO contains PACKAGE
```
```
        DEV       QA      PROD
        ---       --      ----
Deploy  4
svc     4
pv      4
pvc     4
cm      4
secrets 4
→ Aditional 10 resources all together
Let 20 yaml files
perform CRUD 
- DEV 20 commands
- QA 20 commands
- PROD 20 commands 
→ Over all 60 Commands we need to execute
Hard to Update,Remove... bcs we may miss few files
```
```
Packaging Everything together

package1 → App1 20 files
package2 → App2 30 files
package3 → App3 50 files

Install/Update/Remove everything together by HELM
```
```
Platform    Client    Package
--------    ------    -------
Kubernetes  Helm      Chart
```