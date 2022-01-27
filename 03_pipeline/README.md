# Aula 03 - Pipelines no Jenkins
[Videoaula]()

Um Jenkinsfile pode ser escrito usando dois tipos de sintaxe - **Declarativa** e **Scripted**.

Pipeline **declarativo** é um recurso mais recente do Jenkins Pipeline que: fornece recursos sintáticos mais ricos em relação à sintaxe de pipeline com script e foi projetado para facilitar a escrita e a leitura do código do Pipeline

**Exemplo de pipeline scripted** 
```
node {
    stage('step1') {
        echo 'step 1'
    }
    stage('step2') {
        echo 'step 2'
    }
    stage('step3') {
        echo 'step 3'
    }
}
```

**Exemplo de pipeline declarative**
```
pipeline {
    agent any
    stages {
        stage('Step1') {
            steps {
                echo 'step 1'
            }
        }
        stage('Step2') {
            steps {
                echo 'step 2'
            }
        }
        stage('Step3') {
            steps {
                echo 'step 3'
            }
        }
    }
}        
```

*Por entende que o pipeline declartivo é o mais indicado, vamos seguir apenas como os exemplos de pipeline declarativo.*


## Parallel In Sequential

Podemos ter estágios onde a execução de um não interfere no outro, quando temos esta situação, podemos executar os estágios de forma paralela.

```
pipeline {
    agent any
    stages {
        stage('Step1') {
            steps {
                echo 'step 1'
            }
        }
        stage('Step2 - Parallel In Sequential') {
            parallel {
                stage('stpe2A') {
                    steps {
                        echo 'step 2 A'
                    }
                }
                stage('stpe2B') {
                    steps {
                        echo 'step 2 B'
                    }
                }                
            }
        }
        stage('Step3') {
            steps {
                echo 'step 3'
            }
        }
    }
}    
```

## Environment
```
pipeline {
    agent any
    environment {
        BRANCH = 'Master'
    }
    stages {
        stage('step1') {
            environment {
                OUTRO = 'outro valor'
            }
            steps {
                sh 'printenv'
            }
        }
        stage('step2'){
            steps{
                sh 'printenv'
            }
        }
        
    }    
}

```

## When
```
pipeline {
    agent any
    environment {
        BRANCH = 'Release'
    }
    stages {
        stage('step1') {
            environment {
                OUTRO = 'outro valor'
            }
            steps {
                sh 'printenv'
            }
        }
        stage('step2'){
            steps{
                sh 'printenv'
            }
        }
        stage('step3'){
            when{
                expression { BRANCH ==~ /(Master|Hotfix)/ }
            }
            steps{
                echo "step 3 OK para Master ou Hotfix"
            }
        }
        stage('step4'){
            when{
                expression { BRANCH ==~ /(Release)/ }
            }
            steps{
                echo "step 4 OK para Release"
            }
        }        
        
    }
    
}

```

## Parameter string
```
pipeline {
    agent any
    parameters {
        string(name: 'BRANCH', defaultValue: 'Master', description: 'Branch name')
    }
    stages {
        stage('step1') {
            environment {
                OUTRO = 'outro valor'
            }
            steps {
                sh 'printenv'
            }
        }
        stage('step2'){
            steps{
                sh 'printenv'
            }
        }
        stage('step3'){
            when{
                expression { BRANCH ==~ /(Master|Hotfix)/ }
            }
            steps{
                echo "step 3 OK para Master ou Hotfix"
            }
        }
        stage('step4'){
            when{
                expression { BRANCH ==~ /(Release)/ }
            }
            steps{
                echo "step 4 OK para Release"
            }
        }        
        
    }
    
}

```

## Parameter Choice
```
pipeline {
    agent any
    parameters {
        choice(name: 'BRANCH', choices: ['Master', 'Hotfix', 'Release', 'Develop', 'Feature'], description: 'Branch name')
    }
    stages {
        stage('step1') {
            environment {
                OUTRO = 'outro valor'
            }
            steps {
                sh 'printenv'
            }
        }
        stage('step2'){
            steps{
                sh 'printenv'
            }
        }
        stage('step3'){
            when{
                expression { BRANCH ==~ /(Master|Hotfix)/ }
            }
            steps{
                echo "step 3 OK para Master ou Hotfix"
            }
        }
        stage('step4'){
            when{
                expression { BRANCH ==~ /(Release)/ }
            }
            steps{
                echo "step 4 OK para Release"
            }
        }        
        
    }
    
}
```

## Interative
```
pipeline {
    agent any
    stages {
        stage('Example') {
            input {
                message "Should we continue?"
                ok "Yes, we should."
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
        stage('Step2 - Parallel In Sequential') {
            parallel {
                stage('stpe2A') {
                    steps {
                        echo 'step 2 A'
                    }
                }
                stage('stpe2B') {
                    steps {
                        echo 'step 2 B'
                    }
                }                
            }
        }
        stage('Step3') {
            steps {
                echo 'step 3'
            }
        }
    }
}        


```

