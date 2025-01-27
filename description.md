# Description of the contents of this project

<!-- TOC -->

* [Which are the technologies used in the code](#which-are-the-technologies-used-in-the-code)
* [Deployment](#deployment)
    * [Docker Usage](#docker-usage)
    * [Docker Compose](#docker-compose)
    * [How to deploy in a Kubernetes cluster](#how-to-deploy-in-a-kubernetes-cluster)
        * [With Minikube](#with-minikube)
        * [Without Minikube](#without-minikube)
* [Gradle Configuration](#gradle-configuration)
    * [Plugins](#plugins)
    * [Repositories](#repositories)
    * [Dependencies](#dependencies)
    * [Kotlin](#kotlin)

<!-- /TOC -->

## Which are the technologies used in the code

The technologies that we found in the project are:

* Gradle 7.5.1
* Kotlin 1.7
* HTML 5
* Bootstrap
* Markdown

These technologies can be divided depending on its use:

* To document the project it's being used Markdown. Markdown is a language with plain text syntax for the generation of files that can be accessible for different platforms.
* On the server-side we can found Kotlin with the framework Spring, we use Kotlin here because of its features which can be useful on the develop the app.
* To desing the page has been choosen HTML with Bootstrap which is use to customize the page with Sass.
* Finally for build automation we have Gradle. Gradle is a feature which helps us with the compilation of the code, avoiding overwork.

## Deployment

### Docker Usage

to build the docker image and run the container:

```sh
docker build . -t lab1-git-race -f .\src\main\docker\app.dockerfile

docker run -d -p 8080:8080 --name lab1-git-race-container lab1-git-race
```

To stop the container use:

```sh
docker stop <identifier:hash>
```

### Docker Compose

```sh
docker-compose up -d
```

### How to deploy in a Kubernetes cluster

First of all, ensure you've got a properly configured cluster on your computer.
If you don't have one, you can easily install a local Kubernetes cluster via
[Minikube](https://minikube.sigs.k8s.io/docs/start/).

#### With Minikube

First, start your local cluster if you haven't done so yet:

```bash
minikube start
```

Once you have a running minikube cluster, apply the `deployment.yml` manifest as follows
(ensure your current directory is at the root of the project):

```bash
minikube kubectl -- apply -f .\deployment.yaml
```

**Note:** Wait until the pods are on a `Running` state. You can check your cluster status with the following command:

```bash
minikube kubectl get all
```

Finally, expose the service to the browser with the following command:

```bash
minikube service lab1-git-race
```

This will open a tunnel and print a URL that allows you to open the app with your browser of choice.

#### Without Minikube

First check if you can use kubectl on your cluster:

```bash
kubectl --help
```

Once your cluster is up and running, apply the `deployment.yml` manifest as follows
(ensure your current directory is at the root of the project):

```bash
kubectl apply -f ./deployment.yml
```

**Note:** Wait until the pods are on a `Running` state. You can check your cluster status with the following command:

```bash
kubectl get all
```

After that, the service needs to be exposed via port forwarding, so you can open the app with your browser of choice:

```bash
kubectl port-forward svc/lab1-git-race [forwarded-port]:8080
```

For example, if you choose to forward port 3000, the output of the command should be as follows:

```bash
$ kubectl port-forward svc/lab1-git-race 3000:8080
> Forwarding from 127.0.0.1:3000 -> 8080
> Forwarding from [::1]:3000 -> 8080
...
```

If you want to clean the cluster, enter the following command:

```bash
kubectl delete -f deployment.yml
```

## Gradle Configuration

Gradle is a tool for automating building.  
The Gradle build file `build.gradle.kts` (which is located in the main directory) specifies Gradle's configuration for this project.

The build file consists of 4 main sections:

* Plugins
* Repositories
* Dependencies
* Kotlin
* Kotlin compiler options

### Plugins

Plugins are extensions that add features to Gradle.
In this project there are plugins added for [Kotlin](https://github.com/JetBrains/kotlin) and [SpringBoot](https://github.com/spring-projects/spring-boot).

### Repositories

In this section the repository for solving dependencies is declared.  
In our case, `mavenCentral()` specifies that the [Maven Central public repository](https://repo.maven.apache.org/maven2/) will be used to solve dependencies.

### Dependencies

This section contains the dependencies of the project which will be downloaded from Maven's repository specified above.  
The dependencies used in this project are:

* [SpringBoot](https://github.com/spring-projects/spring-boot)
* [Jackson](https://github.com/FasterXML/jackson)
* [Kotlin Reflection](https://kotlinlang.org/docs/reflection.html#jvm-dependency)
* Kotlin Standard Library JDK 8 extension
* [Bootstrap WebJar](https://github.com/webjars/bootstrap)

### Kotlin

Kotlin is configured to run on the JVM.
When the Kotlin targets the JVM platform, options of the compile task are specified in the `compileKotlin` variable.
In our case, we specify that the target version of the JVM is 11 with `jvmTarget` and we configure the compiler to generate error by adding the `-Xjsr305=strict` flag.
