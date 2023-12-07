# HACKATHON
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fake Account Detector</title>
</head>
<body>
    <h1>Fake Account Detector</h1>
    
    <form method="post" action="{% url 'detect_fake_account' %}">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Detect Fake Account</button>
    </form>

    <div id="result"></div>

    <script>
        document.querySelector('form').addEventListener('submit', function (event) {
            event.preventDefault();

            // Implement form data collection and submission here
            // Placeholder AJAX call, replace with actual API request
            fetch('{% url "detect_fake_account" %}', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'X-CSRFToken': document.querySelector('input[name="csrfmiddlewaretoken"]').value
                },
                body: JSON.stringify({
                    username: document.querySelector('#id_username').value,
                    profile_picture_url: document.querySelector('#id_profile_picture_url').value,
                    posting_pattern: document.querySelector('#id_posting_pattern').value,
                    friends_followers_ratio: parseFloat(document.querySelector('#id_friends_followers_ratio').value),
                    activity_history: document.querySelector('#id_activity_history').value,
                })
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById('result').innerHTML = `Result: ${data.is_fake ? 'Fake' : 'Not Fake'}, Confidence: ${data.confidence}, Additional Info: ${data.additional_info}`;
            })
            .catch(error => console.error('Error:', error));
        });
    </script>
</body>
</html>
