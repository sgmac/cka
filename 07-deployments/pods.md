# Deployment Confiugration Pod Template

- **imagePullPolicy** IfNotPresent
- **terminationMessagePath** where to output success or failure information of a container.
- **restartPolicy** Never, Always
- **securityContext** pass one or more seucirty settings, such as selinux, AppArmor, users and UIDs.
- **terminationGracePeriodSeconds** time to wait for a sigterm to run until SIGKILL is used to terminate the container.
