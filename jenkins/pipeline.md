# Pipeline

## Example of pipeline

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
