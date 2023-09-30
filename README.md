npm install express body-parser
- /src
  - server.js
  - routes.js
  - controllers.js
- package.json
- .gitignore
- README.md
const express = require('express');
const bodyParser = require('body-parser');
const routes = require('./routes');

const app = express();
const PORT = process.env.PORT || 3000;

app.use(bodyParser.json());
app.use('/api', routes);

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
const express = require('express');
const router = express.Router();
const controllers = require('./controllers');

router.get('/doctors', controllers.getDoctors);
router.get('/doctors/:doctorId', controllers.getDoctor);
router.post('/appointments', controllers.bookAppointment);

module.exports = router;
const doctors = [
  { id: 1, name: 'Dr. Smith', schedule: 'Monday evenings' },
];

const appointments = [];

const getDoctors = (req, res) => {
  res.json(doctors);
};

const getDoctor = (req, res) => {
  const doctorId = parseInt(req.params.doctorId);
  const doctor = doctors.find(d => d.id === doctorId);
  if (doctor) {
    res.json(doctor);
  } else {
    res.status(404).json({ message: 'Doctor not found' });
  }
};

const bookAppointment = (req, res) => {
  const { doctorId, patientName } = req.body;
  const doctor = doctors.find(d => d.id === doctorId);
  if (doctor) {
    res.json({ message: 'Appointment booked successfully' });
  } else {
    res.status(404).json({ message: 'Doctor not found' });
  }
};

module.exports = {
  getDoctors,
  getDoctor,
  bookAppointment,
};
