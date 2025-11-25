pipeline {
    agent any

    stages {

        stage('Build Metrics Image') {
            steps {
                sh 'docker build -t delivery_metrics .'
            }
        }

        stage('Run Metrics App') {
            steps {
                sh 'docker rm -f delivery-metrics || true'
                sh 'docker run -d --name delivery-metrics -p 8000:8000 delivery_metrics'
            }
        }

   stage('Run Prometheus & Grafana') {
    steps {
        sh '''
            docker rm -f prometheus || true
            docker rm -f grafana || true

            docker run -d --name prometheus \
              --network=host \
              -v $WORKSPACE/prometheus.yml:/etc/prometheus/prometheus.yml \
              -v $WORKSPACE/alert_rules.yml:/etc/prometheus/alert_rules.yml \
              prom/prometheus

            docker run -d --name grafana \
              -p 3000:3000 grafana/grafana
        '''
    }
}

    }
}
