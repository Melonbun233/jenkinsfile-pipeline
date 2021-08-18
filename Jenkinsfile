#!groovy

/*
The MIT License
Copyright (c) 2015-, CloudBees, Inc., and a number of other of contributors
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.
        THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
*/

pipeline {
    agent any

    stages {
        stage('Try to retrive Artifact') {
            steps {
                echo "Current build number: ${BUILD_NUMBER}. Previously: ${currentBuild.previousBuild.number}"
                copyArtifacts filter: 'test_artifact.yaml', projectName: '${JOB_NAME}', selector: specific(currentBuild.previousBuild.number), optional: true
                read_artifact()
            }
        }
        stage('Create Artifact and save') {
            steps {
                save_artifact()
            }
        }
    }
}

def save_artifact () {
    test_results = [:]
    test_results["artifact"] = "artifact value"

    writeYaml(file: "test_artifact.yaml", data: test_results)
    archiveArtifacts artifacts: "test_artifact.yaml"
    echo 'Saving Artifact..'
}

def read_artifact() {
    if (env.BUILD_NUMBER == '1') {
        echo "First build, not expect artifact"
    } else if (fileExists("test_artifact.yaml")){
        echo "test_artifact copied"
    } else {
        echo "test_artifact not found"
    }
}
