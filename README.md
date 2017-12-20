# Getting started with Jenkins2

#### Links

- [github training materials](https://github.com/g0t4)
- [jenkins2-course-spring-boot](https://github.com/g0t4/jenkins2-course-spring-boot)
- [course github-links](https://git.io/vKSVZ)
- [groovy docs](http://docs.groovy-lang.org/docs/latest/html/documentation/)
- [pipeline comp plugins](https://github.com/jenkinsci/pipeline-plugin/blob/master/COMPATIBILITY.md)
- [plugins list](https://plugins.jenkins.io/)
- [jenkins hardware recommendations](https://go.cloudbees.com/docs/cloudbees-documentation/cookbook/book.html#_hardware_recommendations)
- [cookbook](https://go.cloudbees.com/docs/cloudbees-documentation/cookbook/book.html)
- [jenkins.io](https://jenkins.io/)

#### Notes

- local installation

  **default port:** 8080

  **user:** tommic

  **pass:** tommic123

  **admin hash code:** 45118e14c0804697a7fe1d72968b042e


- jobs workspace location

  ```shell
  C:\Users\Tomasz_Michnik\.jenkins\workspace
  ```


- jobs definition

  ```
  c:\Users\Tomasz_Michnik\.jenkins\jobs\<JOB_NAME>\config.xml
  ```

#### Instructions

###### Email notifications

1. Use existing or setup smtp server 

   ```
   docker run --restart unless-stopped --name mailhog -p 1025:1025 -p 8025:8025 -d mailhog/mailhog
   ```

2. Main Menu / Manage Jenkins / Configure System / Extended E-mail Notification

   SMTP server: localhost

   SMTP port: 1025

   Default Content Type: HTML

3. <Some job> / configure / Pipeline; Write and execute method `notify()`

       //calling
       notify('Success')
       
       //method definition
       def notify(status){
       emailext (
         to: "tom.michnik@gmail.com",
         subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
         body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
           <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",)
       }

###### Visualizing test results

1. Pipeline Syntax / Sample Step = General Build Step / Publish Junit test results 

   Test report XML = target/surefire-reports/TEST-*.xml

2. Generate, Copy, Paste to the pipeline

3. Go to <Some_JOB> / <Some_Build> / Test Results

###### HTML Publisher plugin

1. Go to Main Menu / Manage Jenkins / Manage plugins

2. Find, select and install HTML publisher plugin

3. Pipeline Syntax / Sample Step = publishHTML

   HTML directory to archive = target/site/jacoco/

   Index page[s] = index.html

   Keep past HTML reports = YES

   Allow missing report = YES

4. Generate, Copy, Paste to the pipeline

###### Adding Agent

1. Go to Main Menu / Manage Jenkins / Manage Nodes / New Node

   name = agent1

   nr of executors = 2

   Remote root directory = /tmp/jenkins-agent1

   **In Jenkins configuration setting set: TCP port for JNLP agents = Fixed 5000** 

2. Copy slave.jar

3. launch agent from browser or use command

   ```
   java -jar slave.jar -jnlpUrl http://localhost:8080/computer/agent1/slave-agent.jnlp -secret ee210a6b57019bd1a464a571c6f5880dab65b83d7ca9e4fbe2709c3ed98143c1
   ```

4. In a pipeline use agent's label

   ```
   node('win'){...}
   ```

###### Pipeline script from git

1. Create Jenkinsfile and copy you script to the file

2. Push the file to the repository

3. Go to <Your_job> / Pipeline 

   Definition = Pipeline script from SCM

   SCP = link to repo

#### Commands

- running jenkins

  ```shell
  java -jar jenkins.war
  ```


- running local email server inside docker

  ```
  docker run --restart unless-stopped --name mailhog -p 1025:1025 -p 8025:8025 -d mailhog/mailhog
  ```
