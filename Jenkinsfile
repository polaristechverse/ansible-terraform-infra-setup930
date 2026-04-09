pipeline{
    agent{
        label 'Dev'
    }
    parameters {
        choice(name: 'PACKER_BUILD', choices: ['no', 'yes'], description: 'Choose an action')
        string(name: 'REGION', defaultValue: 'us-east-1', description: 'Provide Region')
    }
    stages{
        stage('git checkout'){
            steps {
                checkout scm
            }
        }
        stage('Checking the software'){
            steps{
                sh '''
                terraform version
                packer version
                ansible --version
                '''
            }
        }
         stage('packer build'){
                when {
                    expression { return params.PACKER_BUILD =='yes'}
                }
            steps{
                packerbuild()
            }
        }
        stage('capture amiid'){
            when{
                expression { return params.PACKER_BUILD == 'yes' }
            }
            steps{
                amicapture(params.REGION)
            }
        }
    }
}