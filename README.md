What is FLux CD, it is similar to ArgoCD but just CLI and gives more granular control. 

it is a CD tool, before fluxcd or argocd. jenkins was used to create cicd pipelines and deployment was done using kubectl appply
commands in a stage or manually after changing the recent docker image in manifests manually or by creating a stage in jenkins for it.

but by using argocd or fluxcd we don't need to do it manually or through jenkins. as soon as manifests are changes in git or the new image is created ( if you enabled flux to monitor new image tags) it will deploy on cluster or update the cluster with the changes. 

So will discuss fluxCD because argocd is easy and there is lot of content on web for argoCD.

what actually happens and how to use fluxCD? 

to use fluxCD first we need to install fluxCLI , it is similar to kubectl or dockercli. to use flux commands we needs fluxCLI. 

once fluxcli is installed now you can run flux commands and install flux too. note: fluxCLI is not the FLUX. 

so after cli is installed , you will run the bootstrap command  which will look like this - 

flux bootstrap github \
  --owner=mrakbg \
  --repository=manifestsforrestart \
  --branch=main \
  --path=clusters/prod \
  --personal


Parameter	Description
--owner	 - GitHub username or org
--repository - 	Git repo name
--branch	- Branch to use (usually main)
--path	- The folder in the repo where manifests reside
--personal - 	Indicates the repo is under a personal GitHub account

bootstrap command will create one namespace named flux-system for it so it can install controllers and everything it needs in that namespace which will run in pod. those controllers are responsible for monitoring and applying manifests in cluster and many more things.

when you run bootstrap commands , flux also created a folder structure and some yaml files and pushes those to repo you gave .

the yaml files which define how often flux syncs and at which namespace everything is created related to flux and it's like yaml files for controllers created in flux-system namesapce.

so all these files will be created under flux-system folder and pushed to git. now if you write any yaml files in the same hierarvchy as flux-system folder or create any folder where you push your manifests. flux will by default monitor and apply them in cluster. 

you can write and push manifests in any folder you want but you will have to mention path in kustomization and lfux related files. so it knows from where to fetach and monitor. 

more to come after i do handson. 
 ---- for readme on github
