properties(
    [
        parameters([
            string(defaultValue: 'backend_img', name: 'IMAGE_NAME', description: ''),
            string(defaultValue: 'backend_cont', name: 'CONTAINER_NAME', description: ''),
            [$class: 'ChoiceParameter', 
                                    choiceType: 'PT_SINGLE_SELECT', 
                                    description: '', 
                                    filterLength: 1, 
                                    filterable: false, 
                                    name: 'ARTIFACT', 
                                    script: [
                                        $class: 'GroovyScript', 
                                        fallbackScript: [
                                            classpath: [], 
                                            sandbox: false, 
                                            script: ""
                                        ], 
                                        script: [
                                            classpath: [], 
                                            sandbox: false, 
                                            script: ''' import groovy.io.FileType
                                                     def list = []
                                                     def dir = new File("/var/lib/jenkins/workspace/Pipeline_Job2_Backend/Artifacts")
                                                     dir.eachFileRecurse (FileType.FILES) {
                                                       if(it.getName().equals("Dockerfile") == false){
                                                       list.add(it.getName())
                                                       }
                                                     }
                                                    return list '''
                                        ]
                                    ]
                                ],
            choice(choices: ['8080', '8081', '8082'], name: 'PORT'),
            choice(choices: ['8080', '8081', '8082'], name: 'PORT_APP')
        ])
    ]
)
node{
    stage('Preparation'){
        
        sh 'docker stop ${CONTAINER_NAME}'
        
        sh 'docker rm ${CONTAINER_NAME}'
        
        sh 'a=1 && val=`expr $BUILD_NUMBER - $a` && docker image rm ${IMAGE_NAME}:${val}'
    }
    
    stage('Build'){
        
        dir('Artifacts'){
            
            sh 'docker build --build-arg ARTIFACT_NAME=${ARTIFACT} --build-arg PORT_NUMBER=${PORT} --tag ${IMAGE_NAME}:${BUILD_NUMBER} .'
        }
    }
    
    stage('Deploy'){
        
        sh 'docker run --name ${CONTAINER_NAME} -d -p ${PORT}:${PORT_APP} ${IMAGE_NAME}:${BUILD_NUMBER}'
    }
}
