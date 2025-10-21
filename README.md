# WPD-practicals-showcasing
I performed all the practicals of Web Page Designing course and uploaded them on this website.

## Contact Form Integration

The website includes a contact form section. Here are the methods to make it functional:

### Method 1: Using FormSpree (Easiest - No Backend Required)

1. **Sign up at [FormSpree](https://formspree.io/)**
   - Create a free account
   - Get your unique form endpoint

2. **Update the form in `index.html`:**
   ```html
   <form class="contact-form" id="contactForm" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
   ```

3. **Benefits:**
   - No server setup required
   - Emails sent directly to your inbox
   - Free tier available
   - Spam protection included

### Method 2: Using EmailJS (Client-side Only)

1. **Sign up at [EmailJS](https://www.emailjs.com/)**

2. **Add EmailJS script before the closing `</body>` tag:**
   ```html
   <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
   <script type="text/javascript">
       (function(){
           emailjs.init("YOUR_PUBLIC_KEY");
       })();
   </script>
   ```

3. **Add form submission handler:**
   ```javascript
   document.getElementById('contactForm').addEventListener('submit', function(event) {
       event.preventDefault();
       
       emailjs.sendForm('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', this)
           .then(function() {
               alert('Message sent successfully!');
               document.getElementById('contactForm').reset();
           }, function(error) {
               alert('Failed to send message. Please try again.');
               console.log('Error:', error);
           });
   });
   ```

### Method 3: Using Netlify Forms (If hosted on Netlify)

1. **Add `netlify` attribute to the form:**
   ```html
   <form class="contact-form" id="contactForm" name="contact" method="POST" data-netlify="true">
       <input type="hidden" name="form-name" value="contact">
       <!-- rest of form fields -->
   </form>
   ```

2. **Netlify automatically handles form submissions** and sends notifications to your email

### Method 4: Custom Backend Solution

If you want to build your own backend:

#### Using Node.js + Express + Nodemailer:

1. **Install dependencies:**
   ```bash
   npm install express nodemailer cors body-parser
   ```

2. **Create `server.js`:**
   ```javascript
   const express = require('express');
   const nodemailer = require('nodemailer');
   const cors = require('cors');
   const bodyParser = require('body-parser');

   const app = express();
   app.use(cors());
   app.use(bodyParser.json());

   app.post('/send-email', async (req, res) => {
       const { name, email, subject, message } = req.body;

       let transporter = nodemailer.createTransport({
           service: 'gmail',
           auth: {
               user: 'your-email@gmail.com',
               pass: 'your-app-password'
           }
       });

       let mailOptions = {
           from: email,
           to: 'your-email@gmail.com',
           subject: `Contact Form: ${subject}`,
           text: `Name: ${name}\nEmail: ${email}\n\nMessage:\n${message}`
       };

       try {
           await transporter.sendMail(mailOptions);
           res.json({ success: true });
       } catch (error) {
           res.status(500).json({ success: false, error: error.message });
       }
   });

   app.listen(3000, () => console.log('Server running on port 3000'));
   ```

3. **Update form submission in `index.html`:**
   ```javascript
   document.getElementById('contactForm').addEventListener('submit', async function(event) {
       event.preventDefault();
       
       const formData = {
           name: document.getElementById('name').value,
           email: document.getElementById('email').value,
           subject: document.getElementById('subject').value,
           message: document.getElementById('message').value
       };

       try {
           const response = await fetch('http://localhost:3000/send-email', {
               method: 'POST',
               headers: { 'Content-Type': 'application/json' },
               body: JSON.stringify(formData)
           });

           if (response.ok) {
               alert('Message sent successfully!');
               this.reset();
           } else {
               alert('Failed to send message. Please try again.');
           }
       } catch (error) {
           alert('Error: ' + error.message);
       }
   });
   ```

### Method 5: Using Google Apps Script (Free)

1. **Create a Google Apps Script** connected to your Gmail
2. **Deploy it as a web app**
3. **Use the deployment URL** as your form action endpoint

Detailed instructions: [Google Apps Script Mail Handler](https://github.com/dwyl/learn-to-send-email-via-google-script-html-no-server)

## Recommended Approach

For beginners: **Use FormSpree or EmailJS** - they're the easiest to set up and require no backend knowledge.

For production: **Use Netlify Forms** if hosting on Netlify, or **build a custom backend** for full control.

## Contact Information

- **Name:** Yash Ajay Magar
- **Email:** yash.magar@example.com
- **GitHub:** [yashmagar01](https://github.com/yashmagar01)
- **Institution:** Government Polytechnic Awasari
- **Roll No:** 24CO137
