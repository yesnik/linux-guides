# Pipeline

## Example of pipeline

Contents of `Jenkinsfile`:

```
pipeline {
    agent any
    
    stages {
        stage("One") {
            steps {
                sh "sleep 3"
            }
        }
        stage("Two") {
            steps {
                sh "sleep 1"
            }
        }
    }
}
```
