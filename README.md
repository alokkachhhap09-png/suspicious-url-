<!DOCTYPE html><html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Suspicious URL Checker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #eef2f7;
            text-align: center;
            padding: 40px 15px;
        }    .box {
        background: white;
        max-width: 450px;
        margin: auto;
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0 4px 12px #cccccc;
    }

    input {
        width: 90%;
        padding: 12px;
        margin: 12px 0;
        border: 1px solid #aaa;
        border-radius: 6px;
    }

    button {
        background: #1769e0;
        color: white;
        border: none;
        padding: 12px 20px;
        border-radius: 6px;
        cursor: pointer;
    }

    #result {
        margin-top: 20px;
        font-weight: bold;
        overflow-wrap: anywhere;
    }
</style>

</head>
<body>
    <div class="box">
        <h1>🔎 URL Checker</h1>
        <p>Check a URL for suspicious signs.</p>    <input id="url" type="text"
           placeholder="Enter website URL">

    <br>

    <button onclick="checkURL()">Check URL</button>

    <div id="result"></div>
    <p><small>This tool checks basic warning signs only.
    It cannot guarantee that a website is safe.</small></p>
</div>

<script>
    function checkURL() {
        const input = document.getElementById("url").value.trim();
        const result = document.getElementById("result");

        if (!input) {
            result.textContent = "Please enter a URL.";
            result.style.color = "orange";
            return;
        }

        let address;

        try {
            address = new URL(
                /^[a-zA-Z][a-zA-Z0-9+.-]*:\/\//.test(input)
                ? input : "https://" + input
            );
        } catch {
            result.textContent = "Invalid URL format.";
            result.style.color = "red";
            return;
        }

        const host = address.hostname;
        const warnings = [];

        if (address.protocol !== "https:") {
            warnings.push("No HTTPS connection");
        }

        if (address.username || address.password) {
            warnings.push("Unusual login information in URL");
        }

        if (/^(http|https):\/\/\d{1,3}(\.\d{1,3}){3}/i.test(input)) {
            warnings.push("Uses an IP address instead of a domain");
        }

        if (host.includes("xn--")) {
            warnings.push("Internationalized domain name");
        }

        if (host.split(".").length > 4) {
            warnings.push("Unusually many subdomains");
        }

        if (input.length > 150) {
            warnings.push("Unusually long URL");
        }

        if (warnings.length > 0) {
            result.style.color = "#b45309";
            result.textContent =
                "⚠️ Suspicious signs found: " + warnings.join(", ") +
                ". This does not prove the website is a scam.";
        } else {
            result.style.color = "#1769a0";
            result.textContent =
                "ℹ️ No basic warning signs detected. This does NOT guarantee the URL is safe.";
        }
    }
</script>

</body>
</html>