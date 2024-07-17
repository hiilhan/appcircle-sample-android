pipeline {
    agent any
    environment {
        // GITHUB_TOKEN = credentials('GITHUB_TOKEN')
        AC_PAT = credentials('ac-pat')
        PROFILE_ID = env.PROFILE_ID
        DIR = 'appcircle-sample-android'
    }
    stages {
        stage('Checkout') {
            steps {
                sh 'git clone https://github.com/hiilhan/appcircle-sample-android.git' // https://$GITHUB_TOKEN@github.com/<your-user>/<your-android-project>.git
                // sh 'echo "sdk.dir=/Users/username/Library/Android/sdk" > $DIR/local.properties'
                // sh 'cat $DIR/local.properties'
            }
        }
        stage('Setup') {
          steps {
            dir(DIR) {
                withCredentials([
					file(credentialsId: 'ac-sample-jks', variable: 'AC_SAMPLE_JKS'),
					file(credentialsId: 'keystore-properties', variable: 'KEYSTORE_PROPERTIES')
				]) {
                    // sh 'export ANDROID_HOME=/Users/username/Library/Android/sdk'
                    sh 'chmod +x ./gradlew'
                    
                    sh 'mkdir keystore'
                    sh 'cp $AC_SAMPLE_JKS keystore/ac-sample.jks'
                    sh 'cp $KEYSTORE_PROPERTIES keystore/keystore.properties'
				}
            }
          }
        }
        stage('Build') {
          steps {
            dir(DIR) {
                // sh 'export ANDROID_HOME=/Users/username/Library/Android/sdk'
                sh './gradlew assembleRelease'
                sh 'find . -type f -name "*.apk"'
            }
          }
        }
        stage('Deploy') {
            steps {
                // TD
                // greet accessToken: hudson.util.Secret.fromString(AC_PAT),
                //       profileID: '${PROFILE_ID}',
                //       appPath: '${DIR}/app/build/outputs/apk/release/app-release-signed.apk',
                //       message: 'Hello from Jenkins to TD Android'
                      
                // EAS
                greet accessToken: hudson.util.Secret.fromString(AC_PAT),
                      entProfileId: '${ENT_PROFILE_ID}',
                      appPath: '${DIR}/app/build/outputs/apk/release/app-release-signed.apk',
                      summary: 'This is a summary.',
                      releaseNote: 'This is a release note.',
                      publishType: '1'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
