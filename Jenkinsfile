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
                // sh 'echo "sdk.dir=/Users/hybrayhem/Library/Android/sdk" > $DIR/local.properties'
                // sh 'cat $DIR/local.properties'
            }
        }
        stage('Setup') {
          steps {
            dir(DIR) {
                // sh 'export ANDROID_HOME=/Users/hybrayhem/Library/Android/sdk'
                sh 'chmod +x ./gradlew'
            }
          }
        }
        // TODO: Import signing credentials
        stage('Build') {
          steps {
            dir(DIR) {
                // sh 'export ANDROID_HOME=/Users/hybrayhem/Library/Android/sdk'
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
                //       appPath: '${DIR}/app/build/outputs/apk/release/app-release-unsigned.apk',
                //       message: 'Hello from Jenkins to TD Android'
                      
                // EAS
                greet accessToken: hudson.util.Secret.fromString(AC_PAT),
                      entProfileId: '${ENT_PROFILE_ID}',
                      appPath: '${DIR}/app/build/outputs/apk/release/app-release-unsigned.apk',
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
