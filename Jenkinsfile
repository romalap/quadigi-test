def free_space() {
    powershell '(Get-PSDrive -name $pwd.drive.name).free / 1GB'
}
def rand_num_com = 'Get-Random -Maximum 100'

def flag = false

pipeline {
    agent none
    stages {
        stage('Parallel Stages') {
            failFast true
            parallel {

                stage('print Jenkins build number') {
                    agent {
                        label "windows"
                    }
                    steps {
                        bat 'echo Jenkins build number is %BUILD_NUMBER%'
                    }
                }
                
                stage('generate random num') {
                    agent {
                        label "windows"
                    }
                    steps {
                        script {
                            def rand_num1 = powershell(returnStdout: true, script: rand_num_com).trim()
                            rand_num = rand_num1
                            flag = true
                        }
                    }
                }

                stage('free disk space') {
                    agent {
                        label "windows"
                    }
                    steps {
                        script {
                            free_space()
                         }
                    }
                }

                stage('print random num') {
                    agent {
                        label "linux"
                    }
                    steps {
                        script {
                            waitUntil {
                            flag
                            }
                        }
                    sh "echo 'Random number is ${rand_num}'"
                    }
                }
                
            }
        }
    }
}