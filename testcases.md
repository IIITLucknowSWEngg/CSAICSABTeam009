**Spotify Clone-Test Cases**

### **Functional Test Cases**

---

#### **1. Listener Registration - Register new listener account with valid data**

**Given:**  
The listener is on the registration page.

**When:**  
The listener enters valid email, password, and other details.

**Then:**  
The listener sees a success message: "Registration successful".  
The listener is redirected to the login page (/login).

**Chai.js Code:**
```javascript
describe('Spotify Clone - Listener Registration', () => {
  it('should register the listener successfully', () => {
    regPage.open();
    regPage.register('test@test.com', 'Password123');
    expect(regPage.getSuccessMessage()).to.equal('Registration successful');
    expect(browser.getUrl()).to.include('/login');
  });
});
```

---

#### **2. Listener Registration - Register new listener account with invalid data**

**Given:**  
The listener is on the registration page.

**When:**  
The listener enters invalid email or password.

**Then:**  
The listener sees an error message: "Invalid data".  
The listener is not registered.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Listener Registration', () => {
  it('should not register with invalid data', () => {
    regPage.open();
    regPage.register('', '');
    expect(regPage.getErrorMessage()).to.equal('Invalid data');
    expect(browser.getUrl()).to.not.include('/login');
  });
});
```

---

#### **3. Listener Login - Log in to the listener account with valid data**

**Given:**  
The listener is on the login page.

**When:**  
The listener enters valid credentials.

**Then:**  
The listener sees a welcome message: "Welcome back".  
The listener is redirected to the dashboard (/dashboard).

**Chai.js Code:**
```javascript
describe('Spotify Clone - Listener Login', () => {
  it('should log in successfully with valid credentials', () => {
    loginPage.open();
    loginPage.enterCredentials('test@test.com', 'Password123');
    loginPage.submitLogin();
    expect(loginPage.getWelcomeMessage()).to.include('Welcome back');
    expect(browser.getUrl()).to.include('/dashboard');
  });
});
```

---

#### **4. Listener Login - Log in to the listener account with invalid data**

**Given:**  
The listener is on the login page.

**When:**  
The listener enters invalid credentials.

**Then:**  
The listener sees an error message: "Invalid credentials".  
The listener is not logged in.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Listener Login', () => {
  it('should not log in with invalid credentials', () => {
    loginPage.open();
    loginPage.enterCredentials('invalid@test.com', 'wrongpassword');
    loginPage.submitLogin();
    expect(loginPage.getErrorMessage()).to.equal('Invalid credentials');
    expect(browser.getUrl()).to.not.include('/dashboard');
  });
});
```

---

#### **5. Playlist Management - Create a new playlist**

**Given:**  
The premium listener is logged in and on the dashboard.

**When:**  
The premium listener clicks "Create Playlist" and enters a playlist name.

**Then:**  
The playlist is created successfully.  
The premium listener sees a success message: "Playlist created successfully".

**Chai.js Code:**
```javascript
describe('Spotify Clone - Playlist Management', () => {
  it('should create a new playlist successfully', () => {
    dashboardPage.open();
    dashboardPage.createPlaylist('My Favorite Songs');
    expect(dashboardPage.getSuccessMessage()).to.equal('Playlist created successfully');
    expect(dashboardPage.getPlaylistNames()).to.include('My Favorite Songs');
  });
});
```

---

#### **6. Playlist Management - Delete an existing playlist**

**Given:**  
The premium listener is logged in and has at least one playlist.

**When:**  
The premium listener deletes a playlist.

**Then:**  
The playlist is removed from the list.  
The premium listener sees a success message: "Playlist deleted successfully".

**Chai.js Code:**
```javascript
describe('Spotify Clone - Playlist Management', () => {
  it('should delete a playlist successfully', () => {
    dashboardPage.open();
    dashboardPage.deletePlaylist('My Favorite Songs');
    expect(dashboardPage.getSuccessMessage()).to.equal('Playlist deleted successfully');
    expect(dashboardPage.getPlaylistNames()).to.not.include('My Favorite Songs');
  });
});
```

---

#### **7. Music Playback - Play a song**

**Given:**  
The listener is logged in and has a playlist with songs.

**When:**  
The listener clicks "Play" on a song.

**Then:**  
The song starts playing.  
The listener sees the playback controls.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Music Playback', () => {
  it('should play a song successfully', () => {
    playlistPage.open('My Favorite Songs');
    playlistPage.playSong('Song Title');
    expect(playlistPage.getNowPlaying()).to.equal('Song Title');
    expect(playlistPage.isPlaying()).to.be.true;
  });
});
```

---

### **Non-Functional Test Cases (NFRs)**

---

#### **8. Performance - Registration Load Test**

**Given:**  
The application is being tested under high concurrency.

**When:**  
5000 concurrent listeners attempt to register.

**Then:**  
The registration process should be completed in under 2 seconds for 90% of the listeners.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Performance Test', () => {
  it('should handle 5000 concurrent listeners registering', () => {
    const jMeter = require('jmeter');
    const result = jMeter.testRegistrationLoad(5000);
    expect(result.avgResponseTime).to.be.below(2000); // Response time should be below 2 seconds
  });
});
```

---

#### **9. Scalability - Load Test with 5000 Listeners**

**Given:**  
The application is being tested for scalability.

**When:**  
5000 listeners simultaneously log in and access playlists.

**Then:**  
The application should handle 5000 concurrent listeners without crashing or significant performance degradation.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Scalability Test', () => {
  it('should handle 5000 concurrent listeners without performance degradation', () => {
    const loadTest = require('loadtest');
    loadTest.test({ users: 5000, duration: 60 }, (error, result) => {
      expect(error).to.be.null;
      expect(result.totalRequests).to.equal(5000);
    });
  });
});
```

---

#### **10. Security - Secure Listener Data**

**Given:**  
The listener is interacting with the platform and submitting personal data.

**When:**  
The listener registers or logs in with valid credentials.

**Then:**  
All listener data and communication are encrypted, and no sensitive data is exposed.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Security Test', () => {
  it('should securely encrypt listener data during registration and login', () => {
    const listenerData = { email: 'test@test.com', password: 'Password123' };
    const encryptedData = encryptData(listenerData);
    expect(encryptedData).to.not.include(listenerData.password);
  });
});
```

---

#### **11. Availability - 99.99% Uptime Test**

**Given:**  
The application is being tested for availability.

**When:**  
The application is monitored over a period of time (e.g., 24 hours).

**Then:**  
The application should have 99.99% uptime, with minimal downtime.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Availability Test', () => {
  it('should maintain 99.99% uptime', () => {
    const uptime = checkUptime();
    expect(uptime).to.be.greaterThan(99.99);
  });
});
```

---

#### **12. Usability - Listener Interface Test**

**Given:**  
The listener is interacting with the platform.

**When:**  
The listener navigates through the UI.

**Then:**  
The interface should be intuitive and responsive, providing a seamless experience.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Usability Test', () => {
  it('should provide an intuitive listener interface', () => {
    const uiElements = getUIElements();
    expect(uiElements.length).to.be.greaterThan(0);
    expect(uiElements).to.include.members(['play', 'pause', 'next', 'previous']);
  });
});
```

---

#### **13. Maintainability - Code Documentation Test**

**Given:**  
The application’s source code is being reviewed.

**When:**  
The codebase is checked for proper documentation and adherence to best practices.

**Then:**  
The code should be well-documented, with inline comments and clear function names.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Maintainability Test', () => {
  it('should have well-documented codebase', () => {
    const documentationStatus = checkDocumentation('src');
    expect(documentationStatus).to.equal('Complete');
  });
});
```

---

#### **14. Error Handling - Graceful Error Handling**

**Given:**  
The listener interacts with the platform.

**When:**  
An unexpected error occurs (e.g., network failure).

**Then:**  
The system should handle the error gracefully, displaying a user-friendly error message.

**Chai.js Code:**
```javascript
describe('Spotify Clone - Error Handling Test', () => {
  it('should gracefully handle errors', () => {
    const errorHandled = simulateErrorHandling('network');
    expect(errorHandled).to.be.true;
    expect(getErrorMessage()).to.equal('An unexpected error occurred. Please try again later.');
  });
});
```

---
