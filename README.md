# REST-API
npm install express body-parser multer base64mime
const express = require('express');
const bodyParser = require('body-parser');
const multer = require('multer');
const base64mime = require('base64mime');

const app = express();
const port = process.env.PORT || 3000;

// Middleware
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// Multer configuration for file uploads
const upload = multer();

// Route for POST method
app.post('/bfhl', upload.single('file_b64'), (req, res) => {
  // Extract data from the request
  const { data } = req.body;
  const fileBase64 = req.file ? req.file.buffer.toString('base64') : null;

  // Example logic (replace with your actual logic)
  const response = {
    is_success: true,
    user_id: "john_doe_17091999",
    email: "john@xyz.com",
    roll_number: "ABCD123",
    numbers: data.filter(item => !isNaN(item)),
    alphabets: data.filter(item => isNaN(item)),
    highest_lowercase_alphabet: findHighestLowercaseAlphabet(data),
    file_valid: !!req.file,
    file_mime_type: req.file ? base64mime.fromBase64(fileBase64) : null,
    file_size_kb: req.file ? (req.file.size / 1024).toFixed(2) : null
  };

  // Send response
  res.json(response);
});

// Route for GET method
app.get('/bfhl', (req, res) => {
  // Example response for GET method
  res.status(200).json({ operation_code: 1 });
});

// Start server
app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});

// Function to find highest lowercase alphabet
function findHighestLowercaseAlphabet(data) {
  const lowerAlphabets = data.filter(item => typeof item === 'string' && item.length === 1 && item.match(/[a-z]/));
  return [lowerAlphabets.sort()[lowerAlphabets.length - 1]];
}
