# Aula 01 - Instalação do Jenkinks

## Docker run

'''
docker run -d --name=jenkins --rm \
  -v $(pwd)/jenkins_data:/var/jenkins_home/  \
  -p 8080:8080 \
  -p 50000:50000 \
  jenkins/jenkins:2.319.1-lts-jdk11
'''


Para ver a senha inicial do Jenkins podemos fazer de qualquer uma das duas formas a seguir:

- Para pegar a senha olhando os logs

'''
docker logs jenkins
'''

- Para pegar a senha olhando o conteúdo do arquivo initialAdminPassword

'''
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
'''


## Docker-compose

docker-compose.yml
'''
version: '3.7'
services:
 jenkins:
   image: jenkins/jenkins:2.319.1-lts-jdk11
   ports:
   - "8080:8080"
   - "50000:50000" 
   volumes:
   - ./jenkins_data:/var/jenkins_home
'''

Para iniciar
'''
docker-compose up -d
'''

Para ver a senha inicial do Jenkins podemos fazer de qualquer uma das duas formas a seguir:

- Para pegar a senha olhando os logs

'''
docker-compose logs -f  jenkins
'''

- Para pegar a senha olhando o conteúdo do arquivo initialAdminPassword

'''
docker-compose exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
'''


## Pipeline de teste
echo "Hello Jenkins"
