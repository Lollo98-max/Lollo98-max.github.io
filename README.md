# Lollo98-max.github.io

<!DOCTYPE html>
<html>
<head>
    <title>IP Geolocator</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; max-width: 500px; margin: 0 auto; }
        .container { background: #f0f8ff; padding: 20px; border-radius: 15px; }
        input, button { padding: 12px; margin: 8px 0; width: 100%; box-sizing: border-box; }
        button { background: #007bff; color: white; border: none; border-radius: 5px; }
        #results { margin-top: 20px; padding: 15px; background: white; border-radius: 10px; }
        .loading { display: none; text-align: center; color: #007bff; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🌍 IP Geolocator</h1>
        
        <input type="text" id="ipInput" placeholder="Enter IP or leave blank for your IP">
        <button onclick="locateIP()">📍 Locate IP</button>
        
        <div class="loading" id="loading">
            <div>Loading...</div>
        </div>
        
        <div id="results"></div>
    </div>

    <script>
        async function locateIP() {
            const ip = document.getElementById('ipInput').value.trim();
            const resultsDiv = document.getElementById('results');
            const loadingDiv = document.getElementById('loading');
            
            loadingDiv.style.display = 'block';
            resultsDiv.innerHTML = '';
            
            try {
                const ipToCheck = ip || '';
                const response = await fetch(`https://ipapi.co/${ipToCheck}/json/`);
                const data = await response.json();
                
                if (data.error) {
                    resultsDiv.innerHTML = `<div style="color: red;">Error: ${data.reason}</div>`;
                } else {
                    resultsDiv.innerHTML = `
                        <h3>📍 Location Found</h3>
                        <p><strong>IP:</strong> ${data.ip}</p>
                        <p><strong>City:</strong> ${data.city}</p>
                        <p><strong>Region:</strong> ${data.region} (${data.region_code})</p>
                        <p><strong>Country:</strong> ${data.country_name} 🇺🇸</p>
                        <p><strong>ISP:</strong> ${data.org}</p>
                        <p><strong>Coordinates:</strong> ${data.latitude}, ${data.longitude}</p>
                        <p><strong>Timezone:</strong> ${data.timezone}</p>
                    `;
                    
                    // Optional: Show on map
                    if(data.latitude && data.longitude) {
                        resultsDiv.innerHTML += `
                            <br>
                            <a href="https://maps.google.com/?q=${data.latitude},${data.longitude}" 
                               target="_blank" style="color: blue;">
                               📍 View on Google Maps
                            </a>
                        `;
                    }
                }
            } catch (error) {
                resultsDiv.innerHTML = `<div style="color: red;">Error: ${error.message}</div>`;
            }
            
            loadingDiv.style.display = 'none';
        }
        
        // Add event listener for Enter key
        document.getElementById('ipInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                locateIP();
            }
        });
    </script>
</body>
</html>
