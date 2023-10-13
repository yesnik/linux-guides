# Pipeline

## Example of pipeline

Contents of `Jenkinsfile`:

```
pipeline {
    agent any
    
    stages {
        stage('One') {
            steps {
                sh "sleep 3"
            }
        }
        stage('Two') {
            steps {
                sh "sleep 1"
            }
        }
    }
}
```

Syntax:

- `agent any` statement is a declarative syntax that tells Jenkins to allocate an executor on a node and create a workspace for the pipeline.
- `steps` is a declarative syntax that describes the steps to be run in this stage.
