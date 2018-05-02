pipeline {
    agent any
    stages{
        stage('Build'){
			// önceki atifactleri temizler kurulum paketini oluşturur.
            steps {
                sh 'mvn clean package'            
        	}
        	post {
				//artifactlar arşivlenior.
        		success{
        			echo 'Arşivliyor'
        			archiveArtifacs artifacts: '**/target/*.war'
        		}
        	}
		}
        stage('Deploy Staging'){
			//staging (test) alanına deploy işlemini gerçekleştirir.
            steps{
                build job: 'deploy'
            }
        } 

        stage('Deploy Production'){
        	steps{
				//timout anlamlı: eğer 5 gün içerisinde herhangi bir onay ya da red yapılmaz ise
				//otomatik olarak deploy işlemi gerçekleştirilecek.
	        	timeout(time:5, unit:'DAYS'){
	        		input message: 'Canlı ortama deploy işlemini onaylıyor musunuz?'
	        	}

	        	build job: 'deploy-prod'
	        }
			//deploy işlemi başarılı ya da başarısız olma durumuna göre mesaj döndürür
	        post{
	        	success{
	        		echo 'Canlı ortama deploy edildi'
	        	}
	        	failure{
	        		echo 'Hata oluştu ve canlı ortama deploy edilemedi'
	        	}
	        }
        }
    }
}