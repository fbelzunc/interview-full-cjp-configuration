# Decription

The goal of this exercise is to connect an AD server with a CJP environment.

# Start the AD server

Use [this docker repository](https://github.com/fbelzunc/docker-samba-ad-dc) to start a samba application, which simulates an AD server.

Once you clone this repository, I suggest you to perform the following actions:

```
// Build the image
docker build -t samba-ad-dc .

// Run the container
docker run -e "SAMBA_DOMAIN=SAMDOM" -e "SAMBA_REALM=SAMDOM.EXAMPLE.COM" -e "ROOT_PASSWORD=Opoh6quoo5lu" -e "SAMBA_ADMIN_PASSWORD=Mypassword*2017" --name dc1 --dns 127.0.0.1 -d -p 53:53 -p 389:389 -p 88:88 -p 135:135 -p 139:139 -p 138:138 -p 445:445 -p 464:464 -p 3268:3268 -p 3269:3269 samba-ad-dc

// Check the ad controller is up and running and works properly

ldapsearch -LLL -H ldap://127.0.0.1:3268 -s subtree -b "cn=Users,dc=samdom,dc=example,dc=com" -D "cn=Administrator,cn=Users,dc=samdom,dc=example,dc=com" -w "Mypassword*2017" "(& (userPrincipalName=gogo@samdom.example.com)(objectCategory=user))
```

# Licenses

You will be asked for licenses when installing OC and the CM. You can generate a trial license yourself in the URL below

* https://licenses.cloudbees.com/index.html

# Start Operations Center

1. [Download the latest version of Operations Center](https://nectar-downloads.cloudbees.com/cjoc/rolling/war/2.73.2.1/jenkins-oc.war) and start it from command line with `java -DJENKINS_HOME=<PATH_TO_JENKINS_HOME> -jar jenkins-oc.war`.
2. Configure OC to be able to connect with a client master: check *Jenkins Url* under *Manage Jenkins -> Configure System*; check that under *Manage Jenkins -> Configure Global Security* the JNLP port is set-up correctly. Configure now the AD plugin in Operations Center.

References: 

* https://go.cloudbees.com/docs/cloudbees-documentation/cjoc-user-guide/
* [How to diagnose AD integration problems?](https://support.cloudbees.com/hc/en-us/articles/218625237-How-to-diagnose-AD-integration-problems-)
* [Cannot make my AD configuration to work](https://support.cloudbees.com/hc/en-us/articles/235932268-Cannot-make-my-AD-configuration-to-work)

# Start Jenkins Master

1. [Download the latest version of the client masters](https://nectar-downloads.cloudbees.com/cje/rolling/war/2.73.2.1/jenkins.war) and start it from command line with `java -DJENKINS_HOME=<PATH_TO_JENKINS_HOME> -jar jenkins.war`.

# Connect the CM with OC and set-up security

1. From OC main dashboard you can use *Create Item -> Client Master*.
2. In OC go to *Manage Jenkins -> Configure Global Security* and specify that you would like to use *Single Sign-On (security realm and authorization strategy)*
3. Follow and **understand** [RBAC multiple configurations in a CJOC-CJE architecture](https://support.cloudbees.com/hc/en-us/articles/223657648-RBAC-multiple-configurations-in-a-CJOC-CJE-architecture)

References:
* https://go.cloudbees.com/docs/cloudbees-documentation/cjoc-user-guide/#security





