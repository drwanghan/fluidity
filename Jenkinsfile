pipeline {
    agent { 
        docker {
            image "angusgibson/fluidity:xenial-cmake"
            label 'dockerhost'
        } 
    }
    environment {
        MPLBACKEND = 'PS'
        OMPI_MCA_btl = '^openib'
    }
    stages {
        stage('Configuring') {   
            steps { 
                sh './configure --enable-2d-adaptivity' 
            }
        }    
        stage('Building') {       
            steps { 
                sh 'make -j' ;
                sh 'make -j fltools' ;
                sh 'make manual'
            }
        }
        stage('Testing') {       
            steps { 
                sh 'make unittest' ;
                sh 'make THREADS=8 test' ;
                sh 'make THREADS=8 mediumtest'
                junit 'tests/test_result*xml'
            }
        }
    }
}
