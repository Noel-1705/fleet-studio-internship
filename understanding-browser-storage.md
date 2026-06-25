Core Concepts

What is the purpose of browser storage?                     
JavaScrip or JS in short keeps it's variables or memory in the RAM. It means that as long as the page is in use and never closed it'll not lose the data. So as to not loose the data, even after closing the page, browser storage is useful. It stores the data in the browser it self using different storages. It doesn't loose the data even after closing the browser or restarting the browser, can retain login informations, a guest users cart information etcc.

localStorage – What it is, persistence behavior, size limits, common use cases:
LocalStorage as it's name suggests is the use of locally available space to store details about the web page. It persists both even after a refresh of the page and restarting of the entire browser. It's size is mainly limited to betwee 5 to 10 Mega Bytes of storage (might vary depending on the borwser.).The limitation is that everything is stored as a string only and if you want to store any other type of data you have to convert it yourself. Common uses includes storing of UI preferences of the browser, storing the information of cart items of a guest user so they can just order the item if they wish to later.

sessionStorage – What it is, persistence behavior, size limits, common use cases: Session storage as it's name suggests stores the information about the web page that is needed in that session only. It persists in case of a refresh of the page but it is lost forever after the tab is closed and will not come back if you open the same page in another new tab. Every session storage information is unique. It is mainly limited to around 5 Mega Bytes of storage. It is also limited to string only storage and also an accidental closage of an open tab deletes its session storage. It is used for Multi-step checkouts like those first time orders where we have to add all the details, for form data which need only be filled for once etc.

Cookies – Size limits, automatic sending to server, common use cases, security considerations: cookies are small text records to help a server recoganize a browser which had already accessed the server and is now returning to access more data. It's size limit is usually about 4 KiloBytes and usually a domain can have from 50 to over 180 cookies per domain. Cookies are usally attached to the HTTPS request which is sent to the server. It is the only storage type that is sent to the server. It is majorly used for "remember me" login sessions where the browser has to send request to the server side and inorder for the server to know who the browser is cookies are used in every single request along with server side personalisations and for analytics tracking. The security considerations of cookies are specific commands given from server side to stop malicious access or protection against unauthorised and illegal access to the session in progress. The major security commands include HTTPonly- which doesn't allow JS to read the cookies as protection against stealing of session tokens. another one is samesite- which stops the sharing of the cookies cross site. Stopping other users to send instruction to the already existing session in disguise of the original user.

IndexedDB – What it is, when to use it, advantages over localStorage: It is a real transactional No-SQL database which runs inside the browser. It is used when you need to store far more things than strings and a lot of structured data. Also when you need to query/filter/index it. It is far more useful than localstorage as it can store a lot more data compared to the local storage capacity and can store wide variety of data types too.

Cache API – What it is, role in offline-first apps: It is another storage mechanism specifically for HTTP request/reponse, which stores the information of which response to give for the given URL. In offline-first apps, it is useful to interpret the network requests before they leave the browser to check the cached response immediately if the network is unavailable. It is always used in Service Worker Script.

Practical Explanations:
1. Store a key–value pair in localStorage and retrieve it.
    localStorage.setItem('key','value'); is used to set to the local storage.
    localStorage.getItem('key'); is used to retrieve the item from local storage.
    Yes the data in local storage persists even after a refresh of the page.
    Yes the data in local storage persists even after the browser closes.
2. Store a key–value pair in sessionStorage and retrieve it.
    sessionStorage.setItem('key','value'); is used to add a key value pair to the session storage.
    sessionStorage.getItem('key'); is used to retrieve the data from session storage.
    Yes the data in session storage persists even after a page refresh.
    No the data in session storage is immediately lost after the browser is closed and even after opening the same page in a new tab the data cannot be retrieved.
3. Set a cookie manually in the console and view it in the Cookies section.
    document.cookie="data=value; max-age=age; path=define_path"; is used to set the cookie value in the console.
    document.cookie; retrieves the value in console and after refreshing the cookie table the value will be there
    Yes the cookies persists after refresh
    Yes the cookies persists after the browser closes.
4. creating database:
    const openDB = indexedDB.open("students", 1);
    openDB.onupgradeneeded = function(event) {
    const db = event.target.result;
    if (!db.objectStoreNames.contains("class")) {
    db.createObjectStore("class", { keyPath: "id" });
    console.log("Database and table created!");
    }
    };
    checking if it worked:
    openDB.onsuccess = function() {
    console.log("Database is ready to use!");
    };
    openDB.onerror = function() {
    console.log("Error opening database");
    };
    Adding data to the database:
    const openDB = indexedDB.open("students");
    openDB.onsuccess = function(event) {
    const db = event.target.result;
    const transaction = db.transaction("class", "readwrite");
    const store = transaction.objectStore("class");
    const student = {
    id: 1,
    name: "Noel",
    course: "Computer Science",
    gpa: 3.8
    };
    const addRequest = store.add(student);
    addRequest.onsuccess = function() {
    console.log("Student added!");
    console.log("ID:", student.id);
    console.log("Course:",student.course);
    console.log("Name:", student.name);
    };
    addRequest.onerror = function() {
    console.log("Error adding student");
    };
    };

    retrieving data from the database:
    const openDB = indexedDB.open("students");
    openDB.onsuccess = function(event) {
    const db = event.target.result;
    const transaction = db.transaction("class", "readonly");
    const store = transaction.objectStore("class");
    const getRequest = store.get(1);
    getRequest.onsuccess = function() {
    const record = getRequest.result;
    if (record) {
      console.log("Found the student!");
      console.log("Name:", record.name);
      console.log("Course:", record.course);
      console.log("GPA:", record.gpa);
      console.log("Full record:", record);
    } else {
      console.log("No student found with id 1");
    }
    };
    };

    Yes the data persists in IndexedDB after refresh and also after closing browser.

Analysis: 
1. Which storage types persist across sessions?:
    All the other storage types except for the session storage persists across sessions.
2. Which storage types are automatically sent to the server?:
    The cookies storage type are automatically sent to the server with the HTTPS request to the server.
3. Which storage type is most secure for sensitive information, and why?:
    Cookie storage types are the most secure for the sensitive information if it is set using the correct commands from the server sides as it cannot be changed through java script. It is safe because it can stop sending of data through plain HTTP by using secure command, also stop JS from reading cookie information by using HTTPonly command also sstop the access of cookies by another site by the sametime command.
4. Which storage type is best for large datasets?
    For large datasets IndexedDB is the most useful as it has the most storage space accross the all the other storage types. It has a drawback of having to create a table and structure it properly.
5. Which should be avoided for authentication tokens, and why?:
    For authentication tokens local storage and session storage must be avoided as it cannot stop JS from accessing the sensitive information. Also it cannot send data through HTTPS which is secure and uses encoding.

Key Takeaways:
-Browser storage works by having a storage within the browser using the above mentioned storage types, which all are important on it's own and can handle storage efficiently.
-All but session storage only persisits in case of a refresh and browser closage. session storage can only persist in case of a browser refres in case of a browser closage it is deleted entirely. The scope of local storage, IndexedDB and cache storage are over all tabs and windows of the same origin. Session storage is limited to one browser tab of the same origin. Cookies are controlled by the domain and path.
-The best practice for security and efficieny would be to never store sensitive information in local storage or session storage, using httponly and secure cookies for authenthication tokens. Always use https for all communications. Keep only the data you need as it can affect perfomance.