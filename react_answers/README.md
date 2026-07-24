# ReactJS Hands-On Labs Solutions

This document contains the complete and correct source code solutions for all **19 ReactJS Hands-On Labs (HOL)**. Each section contains the component file structures, source codes, styles, and unit tests as specified in the lab manuals.

---

## Table of Contents
1. [HOL 1: myfirstreact](#hol-1-myfirstreact)
2. [HOL 2: StudentApp](#hol-2-studentapp)
3. [HOL 3: scorecalculatorapp](#hol-3-scorecalculatorapp)
4. [HOL 4: blogapp](#hol-4-blogapp)
5. [HOL 5: CohortDetails](#hol-5-cohortdetails)
6. [HOL 6: TrainersApp](#hol-6-trainersapp)
7. [HOL 7: shoppingapp](#hol-7-shoppingapp)
8. [HOL 8: counterapp](#hol-8-counterapp)
9. [HOL 9: cricketapp](#hol-9-cricketapp)
10. [HOL 10: officespacerentalapp](#hol-10-officespacerentalapp)
11. [HOL 11: eventexamplesapp](#hol-11-eventexamplesapp)
12. [HOL 12: ticketbookingapp](#hol-12-ticketbookingapp)
13. [HOL 13: bloggerapp](#hol-13-bloggerapp)
14. [HOL 14: EmployeeThemeContext](#hol-14-employeethemecontext)
15. [HOL 15: ticketraisingapp](#hol-15-ticketraisingapp)
16. [HOL 16: mailregisterapp](#hol-16-mailregisterapp)
17. [HOL 17: fetchuserapp](#hol-17-fetchuserapp)
18. [HOL 18: CohortDetailsTesting](#hol-18-cohortdetailstesting)
19. [HOL 19: GitClientApp](#hol-19-gitclientapp)

---

## HOL 1: myfirstreact

### Project Description
Create a new React application named `myfirstreact` and render a heading that says `"welcome to the first session of React"`.

### File Structure
```text
myfirstreact/
└── src/
    ├── App.js
    └── index.js
```

### Source Code

#### `src/App.js`
```javascript
import React from 'react';

function App() {
  return (
    <div style={{ textAlign: 'center', marginTop: '50px' }}>
      <h1>welcome to the first session of React</h1>
    </div>
  );
}

export default App;
```

#### `src/index.js`
```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

---

## HOL 2: StudentApp

### Project Description
Create a Student Management Portal named `StudentApp` containing three class components: `Home`, `About`, and `Contact`. Invoke all three components in `App.js`.

### File Structure
```text
StudentApp/
└── src/
    ├── Components/
    │   ├── Home.js
    │   ├── About.js
    │   └── Contact.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/Components/Home.js`
```javascript
import React, { Component } from 'react';

class Home extends Component {
  render() {
    return (
      <div>
        <h3>Welcome to the Home page of Student Management Portal</h3>
      </div>
    );
  }
}

export default Home;
```

#### `src/Components/About.js`
```javascript
import React, { Component } from 'react';

class About extends Component {
  render() {
    return (
      <div>
        <h3>Welcome to the About page of the Student Management Portal</h3>
      </div>
    );
  }
}

export default About;
```

#### `src/Components/Contact.js`
```javascript
import React, { Component } from 'react';

class Contact extends Component {
  render() {
    return (
      <div>
        <h3>Welcome to the Contact page of the Student Management Portal</h3>
      </div>
    );
  }
}

export default Contact;
```

#### `src/App.js`
```javascript
import React, { Component } from 'react';
import Home from './Components/Home';
import About from './Components/About';
import Contact from './Components/Contact';

class App extends Component {
  render() {
    return (
      <div style={{ padding: '20px', fontFamily: 'Arial, sans-serif' }}>
        <h1>Student Management Portal</h1>
        <hr />
        <Home />
        <About />
        <Contact />
      </div>
    );
  }
}

export default App;
```

---

## HOL 3: scorecalculatorapp

### Project Description
Create a functional component named `CalculateScore` which accepts `Name`, `School`, `Total`, and `goal` as props, calculates the percentage average score, and displays it in a styled box.

### File Structure
```text
scorecalculatorapp/
└── src/
    ├── Components/
    │   └── CalculateScore.js
    ├── Stylesheets/
    │   └── mystyle.css
    ├── App.js
    └── index.js
```

### Source Code

#### `src/Stylesheets/mystyle.css`
```css
.scorecard {
  border: 2px solid #28a745;
  border-radius: 10px;
  width: 400px;
  margin: 20px auto;
  padding: 0px 0px 20px 0px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.scorecard-header {
  background-color: #007bff;
  color: white;
  padding: 15px;
  margin: 0;
  text-align: center;
}

.scorecard-body {
  padding: 20px;
}

.scorecard-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  font-size: 16px;
}

.scorecard-label {
  font-weight: bold;
  color: #555;
}

.scorecard-value {
  color: #333;
}

.scorecard-result {
  margin-top: 20px;
  padding-top: 15px;
  border-top: 1px solid #ddd;
  font-size: 18px;
  font-weight: bold;
  color: #28a745;
  text-align: center;
}
```

#### `src/Components/CalculateScore.js`
```javascript
import React from 'react';
import '../Stylesheets/mystyle.css';

const CalculateScore = ({ Name, School, Total, goal }) => {
  const average = ((Total / goal) * 100).toFixed(0);

  return (
    <div className="scorecard">
      <h2 className="scorecard-header">Student Scorecard</h2>
      <div className="scorecard-body">
        <div className="scorecard-row">
          <span className="scorecard-label">Name:</span>
          <span className="scorecard-value">{Name}</span>
        </div>
        <div className="scorecard-row">
          <span className="scorecard-label">School:</span>
          <span className="scorecard-value">{School}</span>
        </div>
        <div className="scorecard-row">
          <span className="scorecard-label">Total:</span>
          <span className="scorecard-value">{Total}</span>
        </div>
        <div className="scorecard-row">
          <span className="scorecard-label">Goal:</span>
          <span className="scorecard-value">{goal}</span>
        </div>
        <div className="scorecard-result">
          Average Score is: {average} %
        </div>
      </div>
    </div>
  );
};

export default CalculateScore;
```

#### `src/App.js`
```javascript
import React from 'react';
import CalculateScore from './Components/CalculateScore';

function App() {
  return (
    <div>
      <CalculateScore 
        Name="Shrivalli" 
        School="Trust Matriculation School" 
        Total={280} 
        goal={400} 
      />
    </div>
  );
}

export default App;
```

---

## HOL 4: blogapp

### Project Description
Fetch a list of posts from jsonplaceholder API inside `componentDidMount()` and print them. Handle lifecycle errors using `componentDidCatch()`.

### File Structure
```text
blogapp/
└── src/
    ├── Post.js
    ├── Posts.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/Post.js`
```javascript
class Post {
  constructor(id, title, body) {
    this.id = id;
    this.title = title;
    this.body = body;
  }
}

export default Post;
```

#### `src/Posts.js`
```javascript
import React, { Component } from 'react';
import Post from './Post';

class Posts extends Component {
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      error: null
    };
  }

  loadPosts() {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        return response.json();
      })
      .then(data => {
        // Map payload to Post models
        const postsList = data.slice(0, 10).map(p => new Post(p.id, p.title, p.body));
        this.setState({ posts: postsList });
      })
      .catch(error => {
        this.setState({ error: error.message });
      });
  }

  componentDidMount() {
    this.loadPosts();
  }

  componentDidCatch(error, info) {
    alert(`Error occurred in component: ${error.toString()}`);
    console.error(error, info);
  }

  render() {
    if (this.state.error) {
      return <div style={{ color: 'red' }}>Error: {this.state.error}</div>;
    }
    
    return (
      <div style={{ padding: '20px' }}>
        <h2>Fetched Posts</h2>
        {this.state.posts.map(post => (
          <div key={post.id} style={{ borderBottom: '1px solid #ddd', padding: '10px 0' }}>
            <h3 style={{ color: '#0056b3' }}>{post.title}</h3>
            <p>{post.body}</p>
          </div>
        ))}
      </div>
    );
  }
}

export default Posts;
```

#### `src/App.js`
```javascript
import React from 'react';
import Posts from './Posts';

function App() {
  return (
    <div>
      <Posts />
    </div>
  );
}

export default App;
```

---

## HOL 5: CohortDetails

### Project Description
Create a dashboard of cohorts using CSS Modules styling. Statuses that are `"ongoing"` display header text in green, and all other statuses in blue.

### File Structure
```text
cohorts-app/
└── src/
    ├── Cohort.js
    ├── CohortDetails.js
    ├── CohortDetails.module.css
    ├── App.js
    └── index.js
```

### Source Code

#### `src/Cohort.js`
```javascript
export const CohortData = [
  {
    code: 'INTADMDF10',
    courseName: '.NET FSD',
    startDate: '22-Feb-2022',
    status: 'ongoing',
    coach: 'Aathma',
    trainer: 'Jojo Jose'
  },
  {
    code: 'INTADMDF11',
    courseName: 'Java Cloud FSE',
    startDate: '01-Mar-2022',
    status: 'Scheduled',
    coach: 'Aparna',
    trainer: 'Rahul Dev'
  }
];
```

#### `src/CohortDetails.module.css`
```css
.box {
  width: 300px;
  display: inline-block;
  margin: 10px;
  padding: 10px 20px;
  border: 1px solid black;
  border-radius: 10px;
  font-family: Arial, sans-serif;
  vertical-align: top;
}

.box dt {
  font-weight: 500;
  margin-top: 5px;
  color: #555;
}

.box dd {
  margin-left: 0;
  margin-bottom: 10px;
  font-weight: bold;
}
```

#### `src/CohortDetails.js`
```javascript
import React from 'react';
import styles from './CohortDetails.module.css';

const CohortDetails = ({ cohort }) => {
  const isOngoing = cohort.status.toLowerCase() === 'ongoing';
  const headingColor = isOngoing ? 'green' : 'blue';

  return (
    <div className={styles.box}>
      <h3 style={{ color: headingColor }}>{cohort.code}</h3>
      <dl>
        <dt>Course Name</dt>
        <dd>{cohort.courseName}</dd>
        <dt>Start Date</dt>
        <dd>{cohort.startDate}</dd>
        <dt>Status</dt>
        <dd>{cohort.status}</dd>
        <dt>Coach</dt>
        <dd>{cohort.coach}</dd>
        <dt>Trainer</dt>
        <dd>{cohort.trainer}</dd>
      </dl>
    </div>
  );
};

export default CohortDetails;
```

#### `src/App.js`
```javascript
import React from 'react';
import { CohortData } from './Cohort';
import CohortDetails from './CohortDetails';

function App() {
  return (
    <div style={{ padding: '20px' }}>
      <h1>Cohort Dashboards</h1>
      <hr />
      <div>
        {CohortData.map(cohort => (
          <CohortDetails key={cohort.code} cohort={cohort} />
        ))}
      </div>
    </div>
  );
}

export default App;
```

---

## HOL 6: TrainersApp

### Project Description
Implement navigation routing (`react-router-dom`) with standard path configuration. Include trainer details matching URL `:id` parameters.

### File Structure
```text
TrainersApp/
└── src/
    ├── TrainersList.js
    ├── TrainerDetails.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/TrainersList.js`
```javascript
import React from 'react';
import { Link } from 'react-router-dom';

export const trainers = [
  { id: '1', name: 'Jojo Jose', skill: 'React' },
  { id: '2', name: 'Rahul Dev', skill: 'Java FSD' },
  { id: '3', name: 'Geetha', skill: 'Python/Flask' }
];

const TrainersList = () => {
  return (
    <div>
      <h2>Our Trainers List</h2>
      <ul>
        {trainers.map(t => (
          <li key={t.id} style={{ margin: '10px 0' }}>
            <Link to={`/trainer/${t.id}`} style={{ textDecoration: 'none', color: '#007bff' }}>
              {t.name} (ID: {t.id})
            </Link>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default TrainersList;
```

#### `src/TrainerDetails.js`
```javascript
import React from 'react';
import { useParams, Link } from 'react-router-dom';
import { trainers } from './TrainersList';

const TrainerDetails = () => {
  const { id } = useParams();
  const trainer = trainers.find(t => t.id === id);

  if (!trainer) {
    return <h3>Trainer Not Found!</h3>;
  }

  return (
    <div style={{ border: '1px solid #ccc', padding: '20px', borderRadius: '5px', width: '300px' }}>
      <h3>Trainer Details</h3>
      <p><strong>Name:</strong> {trainer.name}</p>
      <p><strong>Trainer ID:</strong> {trainer.id}</p>
      <p><strong>Primary Skill:</strong> {trainer.skill}</p>
      <Link to="/" style={{ color: 'red' }}>Back to List</Link>
    </div>
  );
};

export default TrainerDetails;
```

#### `src/App.js`
```javascript
import React from 'react';
import { HashRouter as Router, Routes, Route, Link } from 'react-router-dom';
import TrainersList from './TrainersList';
import TrainerDetails from './TrainerDetails';

function App() {
  return (
    <Router>
      <div style={{ padding: '20px', fontFamily: 'Arial, sans-serif' }}>
        <nav style={{ marginBottom: '20px' }}>
          <Link to="/" style={{ marginRight: '15px', textDecoration: 'none', fontWeight: 'bold' }}>Home</Link>
        </nav>
        <hr />
        <Routes>
          <Route path="/" element={<TrainersList />} />
          <Route path="/trainer/:id" element={<TrainerDetails />} />
        </Routes>
      </div>
    </Router>
  );
}

export default App;
```

---

## HOL 7: shoppingapp

### Project Description
Map and render an array representing a cart of order items inside a standard HTML `<table>` layout. Compute and show total cost at the bottom.

### File Structure
```text
shoppingapp/
└── src/
    ├── App.js
    └── index.js
```

### Source Code

#### `src/App.js`
```javascript
import React from 'react';

const cart = [
  { id: 101, product: 'Laptop', price: 45000, quantity: 1 },
  { id: 102, product: 'Wireless Mouse', price: 1200, quantity: 2 },
  { id: 103, product: 'Keyboard', price: 2000, quantity: 1 },
  { id: 104, product: 'Headphones', price: 3500, quantity: 2 }
];

function App() {
  const totalCost = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);

  return (
    <div style={{ padding: '30px', fontFamily: 'Arial, sans-serif' }}>
      <h2>Shopping Cart Details</h2>
      <table border="1" cellPadding="10" cellSpacing="0" style={{ width: '80%', borderCollapse: 'collapse' }}>
        <thead>
          <tr style={{ backgroundColor: '#f2f2f2' }}>
            <th>Product ID</th>
            <th>Product Name</th>
            <th>Price (Rs)</th>
            <th>Quantity</th>
            <th>Subtotal (Rs)</th>
          </tr>
        </thead>
        <tbody>
          {cart.map(item => (
            <tr key={item.id}>
              <td>{item.id}</td>
              <td>{item.product}</td>
              <td>{item.price}</td>
              <td>{item.quantity}</td>
              <td>{item.price * item.quantity}</td>
            </tr>
          ))}
          <tr style={{ fontWeight: 'bold', backgroundColor: '#e9ecef' }}>
            <td colSpan="4" style={{ textAlign: 'right' }}>Total Cost:</td>
            <td>Rs {totalCost}</td>
          </tr>
        </tbody>
      </table>
    </div>
  );
}

export default App;
```

---

## HOL 8: counterapp

### Project Description
Create a stateful counter tracking visitors or entry/exit metrics using state variables.

### File Structure
```text
counterapp/
└── src/
    ├── App.js
    └── index.js
```

### Source Code

#### `src/App.js`
```javascript
import React, { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div style={{ textAlign: 'center', marginTop: '100px', fontFamily: 'sans-serif' }}>
      <h2>Entry/Exit Counter</h2>
      <h1 style={{ fontSize: '72px', margin: '20px 0' }}>{count}</h1>
      <div>
        <button 
          onClick={() => setCount(count + 1)}
          style={{ padding: '10px 20px', fontSize: '18px', marginRight: '10px', backgroundColor: '#28a745', color: 'white', border: 'none', borderRadius: '5px', cursor: 'pointer' }}
        >
          Increment
        </button>
        <button 
          onClick={() => setCount(count > 0 ? count - 1 : 0)}
          style={{ padding: '10px 20px', fontSize: '18px', backgroundColor: '#dc3545', color: 'white', border: 'none', borderRadius: '5px', cursor: 'pointer' }}
        >
          Decrement
        </button>
      </div>
    </div>
  );
}

export default App;
```

---

## HOL 9: cricketapp

### Project Description
Demonstrate array destructuring, rest operators, and player score filtration (>50 runs) using conditional listings.

### File Structure
```text
cricketapp/
└── src/
    ├── App.js
    └── index.js
```

### Source Code

#### `src/App.js`
```javascript
import React from 'react';

const players = [
  { name: 'Sachin Tendulkar', score: 120 },
  { name: 'Virat Kohli', score: 85 },
  { name: 'MS Dhoni', score: 45 },
  { name: 'Rohit Sharma', score: 110 },
  { name: 'Ravindra Jadeja', score: 38 }
];

function App() {
  // Destructuring array elements and capturing remaining using rest spread
  const [firstPlayer, secondPlayer, ...otherPlayers] = players;

  // Filter out players score greater than 50
  const highScoringPlayers = players.filter(p => p.score > 50);

  return (
    <div style={{ padding: '20px', fontFamily: 'Arial' }}>
      <h2>Cricket Match Player Statistics</h2>
      <hr />
      
      <h3>Key Players:</h3>
      <p><strong>Captain / Opener:</strong> {firstPlayer.name} (Runs: {firstPlayer.score})</p>
      <p><strong>Vice-Captain:</strong> {secondPlayer.name} (Runs: {secondPlayer.score})</p>

      <h3>Rest of the Squad:</h3>
      <ul>
        {otherPlayers.map(p => (
          <li key={p.name}>{p.name} - {p.score} runs</li>
        ))}
      </ul>

      <hr />
      <h3>Players scoring half-century or more (Runs &gt; 50):</h3>
      <table border="1" cellPadding="8" cellSpacing="0">
        <thead>
          <tr style={{ backgroundColor: '#f2f2f2' }}>
            <th>Player Name</th>
            <th>Runs</th>
          </tr>
        </thead>
        <tbody>
          {highScoringPlayers.map(p => (
            <tr key={p.name}>
              <td>{p.name}</td>
              <td style={{ color: 'green', fontWeight: 'bold' }}>{p.score}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default App;
```

---

## HOL 10: officespacerentalapp

### Project Description
Render lists of properties dynamically, setting text styles (green/red) depending on whether monthly rental fees exceed `Rs 15,000`.

### File Structure
```text
officespacerentalapp/
└── src/
    ├── App.js
    └── index.js
```

### Source Code

#### `src/App.js`
```javascript
import React from 'react';

const officeSpaces = [
  { id: 1, name: 'DBS Business Center', rent: 12000, address: 'Nungambakkam, Chennai' },
  { id: 2, name: 'Regus Tech Hub', rent: 22000, address: 'OMR, Chennai' },
  { id: 3, name: 'WeWork Office Space', rent: 18000, address: 'Guindy, Chennai' },
  { id: 4, name: 'Spaces Shared Office', rent: 14000, address: 'T-Nagar, Chennai' }
];

function App() {
  return (
    <div style={{ padding: '30px', fontFamily: 'Arial, sans-serif' }}>
      <h2>Office Spaces for Rent</h2>
      <div style={{ display: 'flex', gap: '20px', flexWrap: 'wrap' }}>
        {officeSpaces.map(space => {
          const isHighRent = space.rent > 15000;
          const rentStyle = {
            color: isHighRent ? 'red' : 'green',
            fontWeight: 'bold',
            fontSize: '18px'
          };

          return (
            <div 
              key={space.id} 
              style={{ border: '1px solid #ccc', borderRadius: '8px', padding: '15px', width: '250px', boxShadow: '2px 2px 5px rgba(0,0,0,0.1)' }}
            >
              <h3>{space.name}</h3>
              <p><strong>Address:</strong> {space.address}</p>
              <p>
                <strong>Monthly Rent: </strong>
                <span style={rentStyle}>Rs {space.rent}</span>
              </p>
            </div>
          );
        })}
      </div>
    </div>
  );
}

export default App;
```

---

## HOL 11: eventexamplesapp

### Project Description
Implement increment/decrement event triggers, message alerts, click events, and a Rupees to Euro currency convertor.

### File Structure
```text
eventexamplesapp/
└── src/
    ├── CurrencyConvertor.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/CurrencyConvertor.js`
```javascript
import React, { useState } from 'react';

const CurrencyConvertor = () => {
  const [amount, setAmount] = useState('');
  const [currency, setCurrency] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (currency.trim().toLowerCase() === 'euro') {
      const converted = parseFloat(amount) * 80;
      alert(`Converting to Euro Amount is ${converted}`);
    } else {
      alert('Only "Euro" conversion is supported in this demo.');
    }
  };

  return (
    <div style={{ border: '1px solid #ddd', padding: '20px', width: '350px', borderRadius: '5px', marginTop: '20px' }}>
      <h3 style={{ color: 'green' }}>Currency Convertor!!!</h3>
      <form onSubmit={handleSubmit}>
        <div style={{ marginBottom: '10px' }}>
          <label style={{ display: 'inline-block', width: '100px' }}>Amount:</label>
          <input 
            type="number" 
            value={amount} 
            onChange={(e) => setAmount(e.target.value)} 
            required 
          />
        </div>
        <div style={{ marginBottom: '10px' }}>
          <label style={{ display: 'inline-block', width: '100px', verticalAlign: 'top' }}>Currency:</label>
          <textarea 
            value={currency} 
            onChange={(e) => setCurrency(e.target.value)} 
            rows="1" 
            required 
          />
        </div>
        <button type="submit">Submit</button>
      </form>
    </div>
  );
};

export default CurrencyConvertor;
```

#### `src/App.js`
```javascript
import React, { useState } from 'react';
import CurrencyConvertor from './CurrencyConvertor';

function App() {
  const [counter, setCounter] = useState(0);

  const handleIncrement = () => {
    setCounter(prev => prev + 1);
    alert('Hello! Member1');
  };

  const handleDecrement = () => {
    setCounter(prev => prev - 1);
  };

  const handleSayWelcome = (msg) => {
    alert(msg);
  };

  const handleSynthetic = (e) => {
    alert('I was clicked');
  };

  return (
    <div style={{ padding: '30px', fontFamily: 'Arial, sans-serif' }}>
      <h2>Event Examples Application</h2>
      <hr />
      
      <h1>Counter: {counter}</h1>
      <div style={{ margin: '15px 0' }}>
        <button onClick={handleIncrement} style={{ marginRight: '10px' }}>Increment</button>
        <button onClick={handleDecrement} style={{ marginRight: '10px' }}>Decrement</button>
        <button onClick={() => handleSayWelcome('welcome')} style={{ marginRight: '10px' }}>Say welcome</button>
        <button onClick={handleSynthetic}>Click on me</button>
      </div>

      <hr />
      <CurrencyConvertor />
    </div>
  );
}

export default App;
```

---

## HOL 12: ticketbookingapp

### Project Description
Build a conditional page layout. Guest users can see the Flight Details. Logged-in users can book flight tickets, showing distinct views when toggling between login and logout actions.

### File Structure
```text
ticketbookingapp/
└── src/
    ├── Components/
    │   ├── LoginButton.js
    │   ├── LogoutButton.js
    │   ├── UserGreeting.js
    │   ├── GuestGreeting.js
    │   └── Greeting.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/Components/LoginButton.js`
```javascript
import React from 'react';

function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

export default LoginButton;
```

#### `src/Components/LogoutButton.js`
```javascript
import React from 'react';

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}

export default LogoutButton;
```

#### `src/Components/UserGreeting.js`
```javascript
import React from 'react';

function UserGreeting() {
  return (
    <div>
      <h2>Welcome back</h2>
      <div style={{ border: '2px solid green', padding: '15px', width: '300px', borderRadius: '5px', marginTop: '10px' }}>
        <h4>Book Flight Tickets</h4>
        <p>Book your domestic flights easily.</p>
        <button onClick={() => alert('Booking Successful!')}>Book Now</button>
      </div>
    </div>
  );
}

export default UserGreeting;
```

#### `src/Components/GuestGreeting.js`
```javascript
import React from 'react';

const flights = [
  { id: 'AI-101', route: 'Chennai to Bangalore', price: 'Rs 5000' },
  { id: 'AI-102', route: 'Mumbai to Delhi', price: 'Rs 7000' }
];

function GuestGreeting() {
  return (
    <div>
      <h2>Please sign up.</h2>
      <h4>Flight Details (Browse Only):</h4>
      <table border="1" cellPadding="8" cellSpacing="0" style={{ width: '350px' }}>
        <thead>
          <tr style={{ backgroundColor: '#f2f2f2' }}>
            <th>Flight</th>
            <th>Route</th>
            <th>Price</th>
          </tr>
        </thead>
        <tbody>
          {flights.map(f => (
            <tr key={f.id}>
              <td>{f.id}</td>
              <td>{f.route}</td>
              <td>{f.price}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default GuestGreeting;
```

#### `src/Components/Greeting.js`
```javascript
import React from 'react';
import UserGreeting from './UserGreeting';
import GuestGreeting from './GuestGreeting';

function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

export default Greeting;
```

#### `src/App.js`
```javascript
import React, { useState } from 'react';
import Greeting from './Components/Greeting';
import LoginButton from './Components/LoginButton';
import LogoutButton from './Components/LogoutButton';

function App() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const handleLoginClick = () => {
    setIsLoggedIn(true);
  };

  const handleLogoutClick = () => {
    setIsLoggedIn(false);
  };

  return (
    <div style={{ padding: '30px', fontFamily: 'Arial, sans-serif' }}>
      <h1>Flight Ticket Reservation</h1>
      <hr />
      <div style={{ margin: '15px 0' }}>
        {isLoggedIn ? (
          <LogoutButton onClick={handleLogoutClick} />
        ) : (
          <LoginButton onClick={handleLoginClick} />
        )}
      </div>
      <Greeting isLoggedIn={isLoggedIn} />
    </div>
  );
}

export default App;
```

---

## HOL 13: bloggerapp

### Project Description
Demonstrate conditional rendering configurations using 3 columns (Book Details, Blog Details, Course Details) separated by solid green vertical borders.

### File Structure
```text
bloggerapp/
└── src/
    ├── data.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/data.js`
```javascript
export const books = [
  { id: 101, bname: 'Master React', price: 670 },
  { id: 102, bname: 'Deep Dive into Angular 11 ', price: 800 },
  { id: 103, bname: 'Mongo Essentials', price: 450 }
];

export const courses = [
  { id: 1, cname: 'Angular', startdate: '4/5/2021' },
  { id: 2, cname: 'React', startdate: '6/3/2021' }
];

export const blogs = [
  { id: 201, topic: 'React Learning', author: 'Stephen Biz', desc: 'Welcome to learning React!' },
  { id: 202, topic: 'Installation', author: 'Schewzdenier', desc: 'You can install React from npm.' }
];
```

#### `src/App.js`
```javascript
import React from 'react';
import { books, courses, blogs } from './data';

function App() {
  // 1. Conditional Rendering for Book Details
  const bookdet = (
    <ul style={{ listStyle: 'none', padding: 0 }}>
      {books.map((book) => (
        <div key={book.id} style={{ marginBottom: '15px' }}>
          <h3>{book.bname}</h3>
          <h4>{book.price}</h4>
        </div>
      ))}
    </ul>
  );

  // 2. Conditional Rendering for Blog Details
  const content = (
    <ul style={{ listStyle: 'none', padding: 0 }}>
      {blogs.map((blog) => (
        <div key={blog.id} style={{ marginBottom: '15px' }}>
          <h3>{blog.topic}</h3>
          <p><strong>{blog.author}</strong></p>
          <p style={{ color: '#555' }}>{blog.desc}</p>
        </div>
      ))}
    </ul>
  );

  // 3. Conditional Rendering for Course Details
  const coursedet = (
    <ul style={{ listStyle: 'none', padding: 0 }}>
      {courses.map((course) => (
        <div key={course.id} style={{ marginBottom: '15px' }}>
          <h3>{course.cname}</h3>
          <p>{course.startdate}</p>
        </div>
      ))}
    </ul>
  );

  return (
    <div style={{ padding: '30px', fontFamily: 'Arial, sans-serif' }}>
      <div style={{ display: 'flex', justifyContent: 'space-between' }}>
        
        {/* Course Details (Left) */}
        <div style={{ flex: 1, padding: '0 20px', textAlign: 'center' }}>
          <h1 style={{ borderBottom: '2px solid #ddd', paddingBottom: '10px' }}>Course Details</h1>
          {coursedet}
        </div>

        {/* Book Details (Center) with vertical borders */}
        <div style={{ flex: 1, padding: '0 20px', textAlign: 'center', borderLeft: '5px solid green', borderRight: '5px solid green' }}>
          <h1 style={{ borderBottom: '2px solid #ddd', paddingBottom: '10px' }}>Book Details</h1>
          {bookdet}
        </div>

        {/* Blog Details (Right) */}
        <div style={{ flex: 1, padding: '0 20px', textAlign: 'center' }}>
          <h1 style={{ borderBottom: '2px solid #ddd', paddingBottom: '10px' }}>Blog Details</h1>
          {content}
        </div>

      </div>
    </div>
  );
}

export default App;
```

---

## HOL 14: EmployeeThemeContext

### Project Description
Implement global context state utilizing React's Context API. Let the application adjust button classes (`btn-light` vs `btn-dark`) dynamically.

### File Structure
```text
theme-app/
└── src/
    ├── ThemeContext.js
    ├── EmployeeCard.js
    ├── EmployeesList.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/ThemeContext.js`
```javascript
import { createContext } from 'react';

const ThemeContext = createContext('light');

export default ThemeContext;
```

#### `src/EmployeeCard.js`
```javascript
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

const EmployeeCard = ({ employee }) => {
  const theme = useContext(ThemeContext);
  
  // Choose button color class based on active theme
  const btnClass = theme === 'light' ? 'btn-light' : 'btn-dark';
  
  const buttonStyle = {
    padding: '8px 16px',
    border: 'none',
    borderRadius: '4px',
    cursor: 'pointer',
    backgroundColor: theme === 'light' ? '#e2e3e5' : '#343a40',
    color: theme === 'light' ? '#333' : '#fff'
  };

  return (
    <div style={{ border: '1px solid #ccc', padding: '15px', borderRadius: '5px', width: '200px' }}>
      <h3>{employee.name}</h3>
      <p>Role: {employee.role}</p>
      <button className={btnClass} style={buttonStyle}>
        View Profile
      </button>
    </div>
  );
};

export default EmployeeCard;
```

#### `src/EmployeesList.js`
```javascript
import React from 'react';
import EmployeeCard from './EmployeeCard';

const employees = [
  { id: 1, name: 'John Doe', role: 'System Architect' },
  { id: 2, name: 'Alice Smith', role: 'UX Designer' }
];

const EmployeesList = () => {
  return (
    <div>
      <h3>Registered Employees</h3>
      <div style={{ display: 'flex', gap: '20px' }}>
        {employees.map(emp => (
          <EmployeeCard key={emp.id} employee={emp} />
        ))}
      </div>
    </div>
  );
};

export default EmployeesList;
```

#### `src/App.js`
```javascript
import React, { useState } from 'react';
import ThemeContext from './ThemeContext';
import EmployeesList from './EmployeesList';

function App() {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={theme}>
      <div style={{ 
        padding: '30px', 
        fontFamily: 'Arial', 
        backgroundColor: theme === 'light' ? '#fff' : '#212529',
        color: theme === 'light' ? '#000' : '#fff',
        minHeight: '100vh'
      }}>
        <h2>Theme Context Application</h2>
        <button 
          onClick={toggleTheme} 
          style={{ marginBottom: '20px', padding: '10px 20px', cursor: 'pointer' }}
        >
          Toggle Theme (Current: {theme})
        </button>
        <hr />
        <EmployeesList />
      </div>
    </ThemeContext.Provider>
  );
}

export default App;
```

---

## HOL 15: ticketraisingapp

### Project Description
Create a class-based form named `ComplaintRegister` to register a complaint. Print employee name and a generated ticket transaction ID using an alert box on submit.

### File Structure
```text
ticketraisingapp/
└── src/
    ├── ComplaintRegister.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/ComplaintRegister.js`
```javascript
import React, { Component } from 'react';

class ComplaintRegister extends Component {
  constructor(props) {
    super(props);
    this.state = {
      ename: '',
      complaint: '',
      NumberHolder: Math.floor(Math.random() * 100) + 1
    };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({ [event.target.name]: event.target.value });
  }

  handleSubmit(event) {
    var msg = 'Thanks ' + this.state.ename + '\n Your Complaint was Submitted. \n Transaction ID is: ' + this.state.NumberHolder;
    alert(msg);
    event.preventDefault();
  }

  render() {
    return (
      <div style={{ textAlign: 'center', marginTop: '50px', fontFamily: 'Arial, sans-serif' }}>
        <h2 style={{ color: 'red' }}>Register your complaints here!!!</h2>
        <form onSubmit={this.handleSubmit} style={{ display: 'inline-block', textAlign: 'left', border: '1px solid #ccc', padding: '20px', borderRadius: '5px' }}>
          <div style={{ marginBottom: '15px' }}>
            <label style={{ display: 'inline-block', width: '120px' }}>Name:</label>
            <input 
              type="text" 
              name="ename" 
              value={this.state.ename} 
              onChange={this.handleChange} 
              required 
            />
          </div>
          <div style={{ marginBottom: '15px' }}>
            <label style={{ display: 'inline-block', width: '120px', verticalAlign: 'top' }}>Complaint:</label>
            <textarea 
              name="complaint" 
              value={this.state.complaint} 
              onChange={this.handleChange} 
              rows="4"
              required 
            />
          </div>
          <div style={{ textAlign: 'center' }}>
            <button type="submit" style={{ padding: '8px 20px', cursor: 'pointer' }}>Submit</button>
          </div>
        </form>
      </div>
    );
  }
}

export default ComplaintRegister;
```

#### `src/App.js`
```javascript
import React from 'react';
import ComplaintRegister from './ComplaintRegister';

function App() {
  return (
    <div>
      <ComplaintRegister />
    </div>
  );
}

export default App;
```

---

## HOL 16: mailregisterapp

### Project Description
Create a validated registration form. Enforce conditions: Full name (>=5 chars), Email (must contain @ and .), Password (>=8 chars).

### File Structure
```text
mailregisterapp/
└── src/
    ├── register.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/register.js`
```javascript
import React from 'react';

const validEmailRegex = RegExp(/^[^\s@]+@[^\s@]+\.[^\s@]+$/);

class Register extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      fullName: '',
      email: '',
      password: '',
      errors: {
        fullName: '',
        email: '',
        password: ''
      }
    };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    event.preventDefault();
    const { name, value } = event.target;
    let errors = { ...this.state.errors };

    switch (name) {
      case 'fullName':
        errors.fullName =
          value.length < 5
            ? 'Full Name must be 5 characters long!'
            : '';
        break;
      case 'email':
        errors.email =
          validEmailRegex.test(value)
            ? ''
            : 'Email is not valid!';
        break;
      case 'password':
        errors.password =
          value.length < 8
            ? 'Password must be 8 characters long!'
            : '';
        break;
      default:
        break;
    }

    this.setState({ errors, [name]: value });
  }

  handleSubmit(event) {
    event.preventDefault();
    const { fullName, email, password, errors } = this.state;
    
    const valid = 
      fullName.length >= 5 &&
      validEmailRegex.test(email) &&
      password.length >= 8 &&
      Object.values(errors).every(val => val === '');

    if (valid) {
      alert('Registration Successful!');
    } else {
      alert('Invalid Form - Registration failed.');
    }
  }

  render() {
    const { errors } = this.state;
    return (
      <div style={{ padding: '20px', maxWidth: '400px', margin: '50px auto', fontFamily: 'Arial, sans-serif' }}>
        <h2>Create Account</h2>
        <form onSubmit={this.handleSubmit} noValidate style={{ border: '1px solid #ccc', padding: '20px', borderRadius: '5px' }}>
          <div style={{ marginBottom: '15px' }}>
            <label style={{ display: 'block', marginBottom: '5px' }}>Full Name</label>
            <input 
              type="text" 
              name="fullName" 
              onChange={this.handleChange} 
              noValidate 
              style={{ width: '95%', padding: '8px' }}
            />
            {errors.fullName.length > 0 && <span style={{ color: 'red', fontSize: '12px' }}>{errors.fullName}</span>}
          </div>
          <div style={{ marginBottom: '15px' }}>
            <label style={{ display: 'block', marginBottom: '5px' }}>Email</label>
            <input 
              type="email" 
              name="email" 
              onChange={this.handleChange} 
              noValidate 
              style={{ width: '95%', padding: '8px' }}
            />
            {errors.email.length > 0 && <span style={{ color: 'red', fontSize: '12px' }}>{errors.email}</span>}
          </div>
          <div style={{ marginBottom: '15px' }}>
            <label style={{ display: 'block', marginBottom: '5px' }}>Password</label>
            <input 
              type="password" 
              name="password" 
              onChange={this.handleChange} 
              noValidate 
              style={{ width: '95%', padding: '8px' }}
            />
            {errors.password.length > 0 && <span style={{ color: 'red', fontSize: '12px' }}>{errors.password}</span>}
          </div>
          <button type="submit" style={{ padding: '10px 20px', cursor: 'pointer' }}>Register</button>
        </form>
      </div>
    );
  }
}

export default Register;
```

#### `src/App.js`
```javascript
import React from 'react';
import Register from './register';

function App() {
  return (
    <div>
      <Register />
    </div>
  );
}

export default App;
```

---

## HOL 17: fetchuserapp

### Project Description
Asynchronously fetch user data from `https://api.randomuser.me/` within `componentDidMount()` and render title, full name, and avatar image.

### File Structure
```text
fetchuserapp/
└── src/
    ├── Getuser.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/Getuser.js`
```javascript
import React from 'react';

class Getuser extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      person: null,
      loading: true
    };
  }

  async componentDidMount() {
    try {
      const url = "https://api.randomuser.me/";
      const response = await fetch(url);
      const data = await response.json();
      this.setState({ person: data.results[0], loading: false });
      console.log(data.results[0]);
    } catch (error) {
      console.error("Error fetching user data:", error);
    }
  }

  render() {
    if (this.state.loading) {
      return <div style={{ padding: '20px', fontSize: '18px' }}>Loading random user...</div>;
    }
    const { person } = this.state;
    return (
      <div style={{ padding: '20px', fontFamily: 'Arial, sans-serif', textAlign: 'center' }}>
        <h2 style={{ textTransform: 'capitalize' }}>
          {person.name.title} {person.name.first} {person.name.last}
        </h2>
        <img 
          src={person.picture.large} 
          alt="user profile" 
          style={{ borderRadius: '50%', border: '4px solid #ddd', width: '150px' }}
        />
      </div>
    );
  }
}

export default Getuser;
```

#### `src/App.js`
```javascript
import React from 'react';
import Getuser from './Getuser';

function App() {
  return (
    <div>
      <Getuser />
    </div>
  );
}

export default App;
```

---

## HOL 18: CohortDetailsTesting

### Project Description
Set up unit testing configurations using Enzyme and Adapter packages. Write unit tests evaluating properties, markup extraction (`find()`), and snapshots.

### File Structure
```text
cohorts-app/
└── src/
    ├── setupTests.js
    ├── CohortDetails.test.js
    ├── CohortDetails.js
    └── Cohort.js
```

### Source Code

#### `src/setupTests.js`
```javascript
import '@testing-library/jest-dom';
import { configure } from "enzyme";
import Adapter from "enzyme-adapter-react-16";

configure({ adapter: new Adapter() });
```

#### `src/CohortDetails.test.js`
```javascript
import React from 'react';
import { shallow, mount } from 'enzyme';
import CohortDetails from './CohortDetails';

describe('Cohort Details Component', () => {
  const dummyCohort = {
    code: 'INTADMDF10',
    courseName: '.NET FSD',
    startDate: '22-Feb-2022',
    status: 'ongoing',
    coach: 'Aathma',
    trainer: 'Jojo Jose'
  };

  test('should create the component', () => {
    const wrapper = shallow(<CohortDetails cohort={dummyCohort} />);
    expect(wrapper.exists()).toBe(true);
  });

  test('should initialize the props', () => {
    const wrapper = mount(<CohortDetails cohort={dummyCohort} />);
    expect(wrapper.props().cohort).toEqual(dummyCohort);
  });

  test('should display cohort code in h3', () => {
    const wrapper = mount(<CohortDetails cohort={dummyCohort} />);
    expect(wrapper.find('h3').text()).toContain(dummyCohort.code);
  });

  test('should always render same html', () => {
    const wrapper = shallow(<CohortDetails cohort={dummyCohort} />);
    expect(wrapper).toMatchSnapshot();
  });
});
```

---

## HOL 19: GitClientApp

### Project Description
Demonstrate asynchronous mocking and network mock configurations. Use axios to fetch Git repo details, and assert simulated outputs.

### File Structure
```text
gitclient/
└── src/
    ├── GitClient.js
    ├── GitClient.test.js
    ├── App.js
    └── index.js
```

### Source Code

#### `src/GitClient.js`
```javascript
import axios from "axios";

class GitClient {
  static getRepositories(userName) {
    const url = `https://api.github.com/users/${userName}/repos`;
    return axios.get(url);
  }
}

export default GitClient;
```

#### `src/App.js`
```javascript
import './App.css';
import GitClient from './GitClient';
import { useEffect, useState } from 'react';

function App() {
  const [repositories, setRepositories] = useState([]);

  useEffect(() => {
    GitClient.getRepositories('techiesyed')
      .then(r => setRepositories(r.data))
      .catch(err => console.error("Error loading git repos:", err));
  }, []);

  return (
    <div className="App" style={{ padding: '30px', fontFamily: 'Arial, sans-serif' }}>
      <h1>Git repositories of User - TechieSyed</h1>
      <div style={{ display: 'inline-block', textAlign: 'left', marginTop: '10px' }}>
        {repositories.map(r => (
          <p key={r.name} style={{ margin: '5px 0', borderBottom: '1px solid #eee', paddingBottom: '3px' }}>
            {r.name}
          </p>
        ))}
      </div>
    </div>
  );
}

export default App;
```

#### `src/GitClient.test.js`
```javascript
import axios from 'axios';
import GitClient from './GitClient';

// Mock the whole axios module
jest.mock('axios');

describe('Git Client Tests', () => {
  test('should return repository names for techiesyed', async () => {
    const dummyRepos = [
      { name: 'appscentricsolutions' },
      { name: 'ArrayListDemo' },
      { name: 'ArrayListDemo01' },
      { name: 'AzureDevopsDemoProductsApi' }
    ];

    // Configure mock resolution for axios.get
    axios.get.mockResolvedValue({ data: dummyRepos });

    const response = await GitClient.getRepositories('techiesyed');
    
    // Assert results match dummy records
    expect(response.data).toEqual(dummyRepos);
    expect(response.data[0].name).toBe('appscentricsolutions');
    expect(axios.get).toHaveBeenCalledWith('https://api.github.com/users/techiesyed/repos');
  });
});
```
