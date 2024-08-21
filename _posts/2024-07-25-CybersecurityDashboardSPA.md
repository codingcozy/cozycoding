---
title: "사이버 보안 대시보드 SPA 최고의 기능과 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-25-CybersecurityDashboardSPA_0.png"
date: 2024-07-25 11:45
ogImage: 
  url: /assets/img/2024-07-25-CybersecurityDashboardSPA_0.png
tag: Tech
originalTitle: "Cybersecurity Dashboard SPA"
link: "https://medium.com/aardvark-infinity/cybersecurity-dashboard-spa-97c4ae797654"
isUpdated: true
---




테이블 태그를 Markdown 형식으로 변경해보았어요.


<img src="/assets/img/2024-07-25-CybersecurityDashboardSPA_0.png" />

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cybersecurity Dashboard</title>
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #1c1e29;
            color: #f8f9fa;
            font-family: 'Roboto', sans-serif;
        }
        .navbar {
            background-color: #2a2d3e;
        }
        .container-fluid {
            padding-top: 20px;
        }
        .card {
            background-color: #2a2d3e;
            border: none;
        }
        .card-header {
            background-color: #2a2d3e;
            border-bottom: 1px solid #44475a;
        }
        .table-dark {
            background-color: #1c1e29;
        }
    </style>
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark">
        <a class="navbar-brand" href="#">CyberSecurity Dashboard</a>
    </nav>
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-4">
                <div class="card">
                    <div class="card-header">
                        <h5>Network Traffic</h5>
                    </div>
                    <div class="card-body">
                        <canvas id="trafficChart"></canvas>
                    </div>
                </div>
            </div>
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">
                        <h5>Threat Alerts</h5>
                    </div>
                    <div class="card-body">
                        | Time | Source IP | Destination IP | Type |
                        |------|-----------|----------------|------|
                        |      |           |                |      |
                    </div>
                </div>
            </div>
        </div>
    </div>
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Sample Data for Network Traffic
            const trafficData = {
                labels: ['12:00', '12:05', '12:10', '12:15', '12:20', '12:25'],
                datasets: [{
                    label: 'Network Traffic (MB)',
                    backgroundColor: 'rgba(75, 192, 192, 0.2)',
                    borderColor: 'rgba(75, 192, 192, 1)',
                    data: [5, 10, 5, 2, 20, 30],
                }]
            };
```

```js
            // Configuration for Traffic Chart
            const config = {
                type: 'line',
                data: trafficData,
                options: {
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true,
                                color: '#f8f9fa'
                            }
                        }],
                        xAxes: [{
                            ticks: {
                                color: '#f8f9fa'
                            }
                        }]
                    }
                }
            };
            // Render Traffic Chart
            const trafficChart = new Chart(
                document.getElementById('trafficChart'),
                config
            );
            // Sample Data for Threat Alerts
            const alerts = [
                { time: '12:01', source: '192.168.1.1', destination: '192.168.1.100', type: 'DDoS' },
                { time: '12:10', source: '192.168.1.2', destination: '192.168.1.101', type: 'Malware' },
                { time: '12:15', source: '192.168.1.3', destination: '192.168.1.102', type: 'Phishing' },
            ];
            // Populate Alert Table
            alerts.forEach(alert => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${alert.time}</td>
                    <td>${alert.source}</td>
                    <td>${alert.destination}</td>
                    <td>${alert.type}</td>
                `;
                alertTableBody.appendChild(row);
            });
        });
    </script>
</body>
</html>
```

# 설명


<div class="content-ad"></div>

- HTML 구조: HTML은 대시보드의 기본 구조를 정의하며, 네비게이션 바, 네트워크 트래픽 차트 및 위협 경고를 위한 표를 포함합니다.
- 부트스트랩 통합: 부트스트랩은 반응형이고 현대적인 디자인을 보장하는 스타일링 및 레이아웃에 사용됩니다.
- CSS 스타일링: 추가 CSS 스타일은 외관을 사용자 정의하며, 사이버 보안 애플리케이션에 적합한 어두운 테마에 중점을 둡니다.
- JavaScript 기능: JavaScript는 Chart.js를 사용하여 네트워크 트래픽 차트를 채우고 위협 경고 표를 동적으로 업데이트하는 데 사용됩니다.

# 사용 사례

- 네트워크 모니터링: 네트워크 트래픽을 모니터링하고 실시간으로 잠재적인 위협을 식별합니다.
- 보안 운영 센터 (SOC): SOC 분석가에게 위협 탐지 및 대응을 위한 간소화된 도구를 제공합니다.
- 교육 도구: 사이버 보안 원칙과 실시간 데이터 모니터링을 보여주는 교육 도구.

<div class="content-ad"></div>

- $10,000 - $50,000: 복잡성, 특징 및 백엔드 시스템 통합에 따라 가치가 크게 달라질 수 있습니다.

# 대상 대상층

- 사이버 보안 전문가: SOC 분석가, 네트워크 보안 엔지니어 및 위협 사냥꾼.
- 교육 기관: 사이버 보안 프로그램을 제공하는 학교 및 대학.
- 기업: 네트워크 보안 모니터링 기능을 강화하려는 기관들.

이 예는 UI/UX에 Bootstrap을 활용하고 동적 데이터 시각화 및 위협 경고 기능을 통합한 정교한 사이버 보안 대시보드에 견고한 기반을 제공합니다.