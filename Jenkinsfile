properties([
    parameters([
        // password( name: 'AWS_ACCESS_KEY_ID', 
        //         defaultValue: '', 
        //         description: 'AWS Credentials: AWS access key ID'),
        // password( name: 'AWS_SECRET_ACCESS_KEY', 
        //         defaultValue: '', 
        //         description: 'AWS Credentials: AWS secret access key'),
        string( name: 'AWS_AKI',
                defaultValue: '',
                description: 'The credential id of the aws access key id'),
        string( name: 'AWS_SAK',
                defaultValue: '',
                description: 'The credential id of the aws secret access key'),
        string( name: 'GIT_URL',
                defaultValue: 'https://github.com/radu-marin/milestone-6-k8s',
                description: 'The github repository link'),
        string( name: 'GIT_BRANCH',
                defaultValue: 'main',
                description: 'The github branch link'),
        choice( name: 'ENV', 
                choices: ['DEV/network', 'DEV/EKS', 'global/iam'], 
                description: 'Choose deployment environment (OBS: deploy `network` and `iam` before `EKS`)'),
        choice( name: 'ACTION', 
                choices: ['Apply', 'Destroy'], 
                description: 'Choose the required action for infrastructure')        
    ])
])

//OBS: change to node("linux") if needed
node("linux"){
    stage("APPLY/DESTROY tf env"){
        checkout scm
        withCredentials([string(credentialsId: params.AWS_AKI, variable: 'AWS_ACCESS_KEY_ID'), 
                         string(credentialsId: params.AWS_SAK, variable: 'AWS_SECRET_ACCESS_KEY')]) {
            docker.image('hashicorp/terraform').withRun('-e "AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID" -e "AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY"') { c ->
                docker.image('hashicorp/terraform').inside('--entrypoint ""') {
                    sh '''
                    cd "live/${ENV}"
                    echo "Current working directory is: $(pwd)"
                    terraform init
                    terraform plan -out=plan
                    '''
                }
             }            
        }
    }
}