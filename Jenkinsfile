pipeline {
    agent none
    parameters {
        string(name: 'NODE_LABEL', defaultValue: 'linux', description: 'Node label to run the script on')
        string(name: 'SCRIPT', defaultValue: 'echo "Hello World"', description: 'Script to execute')
    }
    stages {
        stage('Execute') {
            steps {
                script {
                 def nodes = nodesByLabel label: "${params.NODE_LABEL}"
                 nodes = nodes.sort()

                    Map tasks = [:]

                    for (int i = 0; i < nodes.size(); i++) {
                        def label = nodes[i]
                        def stageName = "Execute ${nodes[i]}"
                        tasks[label] = {
                            node(label) {
                                stage(stageName) {
                                      script {
                                          sh "${params.SCRIPT}"
                                      }
                                }
                            }
                        }
                    }

                    timeout(time: 3, unit: 'MINUTES') {
                        parallel(tasks)
                    }
                }
            }
        }
    }
}
