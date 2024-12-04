**Spotify Clone-Test Cases**
| Scenario TID | Scenario Description                                     | Test Case ID | Pre Requisite           | Step to Execute (Action)      | Expected Result                                                                                     | Actual Result                                | Test Status | Executed By | Test Priority |
|--------------|---------------------------------------------------------|--------------|-------------------------|--------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|-------------|-------------|---------------|
| 1            | Register new user account with valid data               | reg001       | email id                | navigate here                 | Registration page should work with valid credentials                                                | Registration is successful                  | Passed      | Tester      | High          |
| 2            | Register new user account with invalid data             | reg001.1     | email id                | navigate here                 | Registration page should not accept invalid credentials                                              | Registration successful with invalid data   | Failed      | Tester      | High          |
| 3            | Log in to the user account with valid data              | log001       | existing account        | navigate to the website       | Login page should work with valid credentials                                                       | Login successful with valid credentials     | Passed      | Tester      | High          |
| 4            | Log in to the user account with invalid data            | log001.1     | existing account        | navigate to the website       | Login should be rejected with invalid credentials                                                   | Login successful with invalid credentials   | Failed      | Tester      | High          |
| 5            | Play a song successfully                                | play001      | logged-in user          | select song and click play    | The selected song should start playing, and now-playing section should update                       | Song playback working as expected           | Passed      | Tester      | High          |
| 6            | Pause a song during playback                           | play001.1    | logged-in user          | click pause                   | The song playback should pause                                                                      | Playback paused successfully                | Passed      | Tester      | Medium        |
| 7            | Create a playlist                                       | playlist001  | logged-in user          | create a new playlist         | Playlist should be created successfully and listed in the user's library                            | Playlist creation working as expected       | Passed      | Tester      | High          |
| 8            | Add songs to a playlist                                 | playlist001.1| logged-in user, playlist| add song to playlist          | The selected song should be added to the playlist                                                   | Song added to the playlist successfully     | Passed      | Tester      | Medium        |
| 9            | Delete a playlist                                       | playlist001.2| logged-in user, playlist| delete the playlist           | The playlist should be removed from the user's library                                              | Playlist deletion working as expected       | Passed      | Tester      | Medium        |
| 10           | Upgrade to premium subscription                         | sub001       | logged-in user          | select plan and pay           | User should be upgraded to premium, and premium features should become accessible                   | Premium upgrade successful                  | Passed      | Tester      | High          |
| 11           | Access premium-only features after upgrade              | sub001.1     | premium user            | use premium feature           | The selected premium-only feature should work                                                       | Premium feature accessible                  | Passed      | Tester      | Medium        |
| 12           | Search for a specific song                              | search001    | logged-in user          | search for a song by title    | The search result should list the song with accurate details                                         | Search results accurate                     | Passed      | Tester      | High          |
| 13           | Search for a non-existent song                          | search001.1  | logged-in user          | search for a song by title    | The search result should display "No results found"                                                 | Displayed "No results found" message        | Passed      | Tester      | Medium        |
### Spotify Clone - User Registration Test  

**Feature:** User Registration  
**Scenario:** Register new user account with valid data  
**Description:**  
Test the user registration process to ensure users can register successfully with valid details.  

**Steps:**  
- **Given:**  
  The user is on the registration page.  
- **When:**  
  The user enters valid email, password, and other details.  
- **Then:**  
  - The user sees a success message: "Registration successful".  
  - The user is redirected to the login page (`/login`).  

**Chai.js Code:**  

```javascript
const expect = chai.expect;

describe('Spotify Clone - User Registration', () => {
  it('should register the user successfully', () => {
    regPage.open();
    regPage.register('test@test.com', 'Password123');
    expect(regPage.getSuccessMessage()).to.equal('Registration successful');
    expect(browser.getUrl()).to.include('/login');
  });
});
```

---

**Scenario:** Register new user account with invalid data  
**Description:**  
Ensure the registration process prevents invalid data submissions.  

**Steps:**  
- **Given:**  
  The user is on the registration page.  
- **When:**  
  The user enters invalid email or password.  
- **Then:**  
  - The user sees an error message: "Invalid data".  
  - The user is not registered.  

**Chai.js Code:**  

```javascript
const expect = chai.expect;

describe('Spotify Clone - User Registration', () => {
  it('should not register with invalid data', () => {
    regPage.open();
    regPage.register('', '');
    expect(regPage.getErrorMessage()).to.equal('Invalid data');
    expect(browser.getUrl()).to.not.include('/login');
  });
});
```

---

### Spotify Clone - User Login Test  

**Feature:** User Login  
**Scenario:** Log in to the user account with valid data  
**Description:**  
Test the login functionality with valid credentials.  

**Steps:**  
- **Given:**  
  The user is on the login page.  
- **When:**  
  The user enters valid credentials.  
- **Then:**  
  - The user sees a welcome message: "Welcome back".  
  - The user is redirected to the dashboard (`/dashboard`).  

**Chai.js Code:**  

```javascript
const expect = chai.expect;

describe('Spotify Clone - User Login', () => {
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

**Scenario:** Log in to the user account with invalid data  
**Description:**  
Ensure login fails with invalid credentials.  

**Steps:**  
- **Given:**  
  The user is on the login page.  
- **When:**  
  The user enters invalid credentials.  
- **Then:**  
  - The user sees an error message: "Invalid credentials".  
  - The user is not logged in.  

**Chai.js Code:**  

```javascript
const expect = chai.expect;

describe('Spotify Clone - User Login', () => {
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

### Spotify Clone - Playlist Management Test  

**Feature:** Playlist Management  
**Scenario:** Create a new playlist  
**Description:**  
Test if users can create a new playlist.  

**Steps:**  
- **Given:**  
  The user is logged in and on the dashboard.  
- **When:**  
  The user clicks "Create Playlist" and enters a playlist name.  
- **Then:**  
  - The playlist is created successfully.  
  - The user sees a success message: "Playlist created successfully".  

**Chai.js Code:**  

```javascript
const expect = chai.expect;

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

**Scenario:** Delete an existing playlist  
**Description:**  
Test if users can delete a playlist.  

**Steps:**  
- **Given:**  
  The user is logged in and has at least one playlist.  
- **When:**  
  The user deletes a playlist.  
- **Then:**  
  - The playlist is removed from the list.  
  - The user sees a success message: "Playlist deleted successfully".  

**Chai.js Code:**  

```javascript
const expect = chai.expect;

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

### Spotify Clone - Music Playback Test  

**Feature:** Music Playback  
**Scenario:** Play a song  
**Description:**  
Test if users can play a song from a playlist.  

**Steps:**  
- **Given:**  
  The user is logged in and has a playlist with songs.  
- **When:**  
  The user clicks "Play" on a song.  
- **Then:**  
  - The song starts playing.  
  - The user sees the playback controls.  

**Chai.js Code:**  

```javascript
const expect = chai.expect;

describe('Spotify Clone - Music Playback', () => {
  it('should play a song successfully', () => {
    playlistPage.open('My Favorite Songs');
    playlistPage.playSong('Song Title');
    expect(playlistPage.getNowPlaying()).to.equal('Song Title');
    expect(playlistPage.isPlaying()).to.be.true;
  });
});
``` 
