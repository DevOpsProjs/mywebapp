node {
   
    stage("Git Clone") {
        git url: 'https://github.com/DevOpsProjs/mywebapp.git', branch: 'master'
    }
    
    stage ("Maven Package") {
        def mavenHome= tool  name: "Maven-3.8.5", type: "maven"  
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage("Build Docker Image") {
        sh "docker build -t sreeni78/mywebapp:${BUILD_NUMBER} ."
    }
    
    stage("Docker Login and Push") {
        withCredentials([usernamePassword(credentialsId: 'Docker_Hub_Creds', passwordVariable: 'Docker_Pass', usernameVariable: 'Docker_User')]) {     
             sh "docker login -u ${Docker_User} -p ${Docker_Pass}"
        }
       
        sh "docker push sreeni78/mywebapp:${BUILD_NUMBER}"
    }
    
    stage("Deploy Container on Docker Server") {	
			sshagent(['Docker_Container']) {
				//sh 'ssh -o StrictHostKeyChecking=no root@192.168.9.42 docker stop mymwa'
				//sh 'ssh -o StrictHostKeyChecking=no root@192.168.9.42 docker rm -f mymwa || true'
				sh 'ssh -o StrictHostKeyChecking=no root@192.168.9.42 docker run -d -p 9090:8080 --name mymwa sreeni78/mywebapp:${BUILD_NUMBER}'
			}
    }
	
	
}
