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
                read_artifact()
            }
        }
        stage('Create Artifact and save') {
            parallel {
                stage('Branch 1') {
                    steps {
                        save_artifact (1)
                    }
                }
                stage('Branch 2') {
                    steps {
                        save_artifact (2)
                    }
                }
                stage('Branch 3') {
                    steps {
                        save_artifact (3)
                    }
                }
                stage('Branch 4') {
                    steps {
                        save_artifact (4)
                    }
                }
            }
        }
    }
}

def save_artifact (int i) {
    test_results = [:]
    test_results["artifact"] = "build number ${currentBuild.number}, branch ${i}"

    writeYaml(file: "test_artifact_${i}.yaml", data: test_results, overwrite: true)
    archiveArtifacts artifacts: "test_artifact_${i}.yaml"
    echo "Saving Artifact on Branch ${i}.."
}

def read_artifact() {
    if (currentBuild.number == '1') {
        echo "First build, not expect artifact"
        return
    } 

    echo ("Current build number: ${currentBuild.number}. Previously: ${currentBuild.previousBuild.number}")
    copyArtifacts (
        filter: 'test_artifact.yaml', 
        fingerprintArtifacts: true,
        projectName: '${JOB_NAME}', 
        selector: specific("lastBuild"), 
        optional: true
    )

    if (fileExists("test_artifact.yaml")){
        test_results = readYaml(file: "test_artifact.yaml")
        echo test_results["artifact"]
        echo "test_artifact copied"
    } else {
        echo "test_artifact not found"
    }
}
