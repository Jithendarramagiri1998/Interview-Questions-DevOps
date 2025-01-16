# Groovy, Jenkins, and Docker Examples

## 1. Difference Between `==` and `===` in Groovy

In Groovy:
- `==` checks for value equality (like `.equals()` in Java).
- `===` checks for reference equality (like `==` in Java).

**Example:**
```groovy
def str1 = "Groovy"
def str2 = new String("Groovy")

println(str1 == str2)  // true: because their values are the same
println(str1 === str2) // false: because they are different objects (references)
```

---

## 2. Handling Errors in Groovy Scripts

You handle errors in Groovy using `try-catch` blocks. This ensures your script continues to run even when an exception occurs.

**Example:**
```groovy
try {
    def result = 10 / 0  // This will cause a division by zero exception
    println("Result: $result")
} catch (ArithmeticException e) {
    println("Error: Division by zero is not allowed.")
} finally {
    println("Execution complete.")
}
```

---

## 3. Parsing JSON in Groovy

You can parse JSON using the `JsonSlurper` class.

**Example:**
```groovy
import groovy.json.JsonSlurper

def jsonContent = '''
{
  "name": "Jithendar",
  "role": "DevOps Engineer",
  "skills": ["AWS", "Groovy", "Docker"]
}
'''

def jsonParser = new JsonSlurper()
def data = jsonParser.parseText(jsonContent)

println("Name: ${data.name}")
println("Role: ${data.role}")
println("First Skill: ${data.skills[0]}")
```

---

## 4. Stages of a Jenkins Pipeline

Jenkins pipelines have several stages:
- **Checkout:** Clone the source code from a version control system.
- **Build:** Compile or package the application.
- **Test:** Run automated tests.
- **Deploy:** Deploy the application to the desired environment.

**Example Pipeline with Multiple Stages:**
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo "Cloning the repository"
                git url: 'https://github.com/example/repo.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo "Building the application"
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                echo "Running tests"
                sh './gradlew test'
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying application"
                sh './deploy.sh'
            }
        }
    }
}
```

---

## 5. Integrating Git with Jenkins

To integrate Git with Jenkins:
1. Install the Git plugin in Jenkins.
2. In a Jenkins job, specify the repository URL under "Source Code Management."

**Example: Git Integration in Jenkins Pipeline**
```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/example/repo.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                echo "Build process starts here"
            }
        }
    }
}
```

---

## 6. Creating a Jenkins Pipeline Script with Multiple Stages

**Example Script:**
```groovy
pipeline {
    agent any
    stages {
        stage('Initialize') {
            steps {
                echo "Initializing pipeline"
            }
        }
        stage('Build') {
            steps {
                echo "Building the application"
            }
        }
        stage('Test') {
            steps {
                echo "Testing the application"
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying to production"
            }
        }
    }
}
```

---

## 7. Concept of Containerization Using Docker

**Containerization** allows you to package applications with all dependencies, ensuring consistent environments across systems.

**Docker** is a tool for containerization, enabling the creation, deployment, and management of containers.

---

## 8. Creating a Docker Image for a Java-Based Web Application

Steps:
1. Write a `Dockerfile`.
2. Build the image using `docker build`.
3. Run the container using `docker run`.

**Example `Dockerfile`:**
```Dockerfile
# Use an official Java runtime as a parent image
FROM openjdk:11-jre-slim

# Set the working directory
WORKDIR /app

# Copy the application JAR file to the container
COPY app.jar app.jar

# Run the application
CMD ["java", "-jar", "app.jar"]
```

To build and run:
```bash
docker build -t java-app .
docker run -p 8080:8080 java-app
```

---

## 9. Automating Docker Deployment to Kubernetes with Groovy

You can use Groovy scripts to automate Docker container deployment to a Kubernetes cluster using `kubectl` commands.

**Example:**
```groovy
pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t java-app .'
            }
        }
        stage('Push to Registry') {
            steps {
                sh 'docker tag java-app your-registry/java-app:latest'
                sh 'docker push your-registry/java-app:latest'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }
    }
}
```

---

## 10. Purpose of a Dockerfile

A `Dockerfile` is a text file containing instructions to build a Docker image. It automates the process of creating Docker containers.

**Simple Example:**
```Dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y curl
CMD ["echo", "Hello from Docker!"]
