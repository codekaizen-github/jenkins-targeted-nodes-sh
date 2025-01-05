pipeline {
    agent none
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
