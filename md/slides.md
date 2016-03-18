# **Project <img src="img/project-link.svg" style="height: .8em; vertical-align: middle;" alt="Project Link" title="Project Link"> Link**<br>Web app

---

[@g_marty](https://twitter.com/g_marty) - Guillaume Marty

I am <img src="img/French.svg" style="height: .8em; vertical-align: middle;" alt="French" title="French"> and living in the <img src="img/UK.svg" style="height: .8em; vertical-align: middle;" alt="UK" title="UK">

I work at <img src="img/Mozilla.svg" style="height: .8em; vertical-align: middle; margin-bottom: 21px;" alt="Mozilla" title="Mozilla"> on the **Connected Devices** team

---

## App

[github.com/fxbox/app](https://github.com/fxbox/app)

> An app for Project Link

---

### Characteristics

* **P**rogressive **W**eb **A**pp
* Built on [MVC](https://github.com/fxos-eng/mvc) and [React](https://facebook.github.io/react/)

Note:
Web manifest, service worker, push notifications (to come).

---

### MVC

[github.com/fxos-eng/mvc](https://github.com/fxos-eng/mvc)

> A simple MVC pattern using extendable ES6 classes

Note:
Developed for Spark. Ultra simple, clean and flexible framework.
Will probably won't be used in the future.
Also uses https://github.com/fxos-eng/build to get watcher, linter installed.

---

### React

[facebook.github.io/react/](https://facebook.github.io/react/)

> A JavaScript library for building user interfaces

Note:
New paradigm in writing UI.
Only for the view.
Virtual DOM for ease and performance.
One-way reactive data flow.
Rationale: Flexibility, commitment from Facebook, community. Likely to stay.

---

#### React components lifecycle

* Mount
* Props change
* Update
* Unmount

---

#### Props and states

* Props are immutable
* States can be mutated

Note:
Props for id, library, constants...
States for user changes, values received from events...

---

## foxbox.js

[github.com/fxbox/app/tree/master/app/js/lib](https://github.com/fxbox/app/tree/master/app/js/lib)

> A library to interact with the Project Link box

Note:
Abstracting away the complexity of each adapter. 3rd party dev-friendly.
Will be independent and live in its own repo in the future.
Can be used to prototype features on the frontend.

---

### Instantiation

```javascript
const foxbox = new Foxbox();
```

---

### Code organisation

3 files:

* `foxbox.js`
* `foxbox-settings.js`
* `foxbox-db.js`

---

### `foxbox.js`

* Abstract communication with the box
* Discovery, authentication, caching
* Handle local / remote connection

---

### `foxbox-settings.js`

* For settings and configuration
* Backed by local storage
* Private to foxbox instance

Note:
All settings have default values.
Local storage is used on initialisation and writing settings.
Value changes can be listened to.

---

### `foxbox-db.js`

* For caching the services
* Backed by indexed db
* Private to foxbox instance

Note:
Cache services for faster startup time.
Implement tags.

---

### Communication foxbox → UI

foxbox emits events:

```javascript
this._dispatchEvent('service-change', services);
```

Note:
service-change, service-data-change, box gets connected/disconnected, connection
becomes local/remote...

---

### Communication UI → foxbox

the UI listens to events and calls foxbox methods:

```javascript
// Listen to events
foxbox.addEventListener('service-change', updateUI);

// Give foxbox some order
foxbox.getServices()
  .then(updateUI);
```

---

### Built-in services caching

1. On startup return cached services
2. Fetch updates asynchronously
3. On updates, emit event

```javascript
foxbox.getServices()
  .then(services => {
    // Cached services
    updateUI(services);
  });

// Listen to updates on services
foxbox.addEventListener('service-change', updateUI);
```

Note:
Faster startup.
UI always in sync with the box.

---

### Iterate faster

Making it easy to:

* Mocking WIP features
* Prototyping new ideas

Note:
WebSockets are polyfilled using polling.
Tags are prototyped purely on the frontend.

---

### Future development 1/3

Frontend adapters for each class of service

```javascript
services.forEach(service => {
  switch (service.type) {
    case 'ipcamera':
      service.getSnapshot();
      break;
    case 'colorlight':
      service.turnOn();
      break;
  }
});
```

Note:
Stateful objects with specific props and methods.

---

### Future development 2/3

Add a unit test suite

---

### Future development 3/3

Need better documentation

---

### Topics to discuss

* Browser version support
* Code organisation
* Better build system
* Library name
