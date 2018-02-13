# Forward JS Notes
**San Francisco - Feb 13 2017**

## Pitching time

### Blockchain
Bitcoins can handle 4 transactions/seconds

Why can't we put everything on the blockchain?

Tierion Network puts your data online and provides a timestamp proof of any data or files or event.

What applications are running on ETH
##### Philips 
Medical applications

##### Distributed Health
first blockchain healthcare conference. 

13 of 18 hackatons team used ETH to develop a system.

### Carbon 
Industrial grade 3d Printers


## Design systems at scale - Adobe
### by Sarah Federman @sarah_federman
Design systems: a collection of products that solve the problem of scaling your design practice.

#### Why ?
- Quick iteration
- stronger brand awareness
- improves user experience
- empowers developer
- everyone speaks the same language
- You have a single point of success of failure
- Removes dupplication

#### Dupplication
- dupplication leads to fragmentation which leads to stop sharing things.
- Code base becomes confusing
- Things get out of date
- a11y = accessibility

#### How did Adobe scale
- Componentizing content
- All content was put into a content type
- This enabled to have JSON tests and schema tests
Check out http://spectrum.corp.adobe.com


### Conversational Interfaces - Slack
#### Tomomi Imura @girlie_mac girliemac.com

- Graphical interfaces are going to fade away and conversational interfaces will take over
- DialogFlow // Motion AI are examples
- AIaaS rise = Artificial Intelligence as a Service

- KLM has a bot to book flight tickets/ check status etc...
- Natural Language Processing Platform: Big players are heavily investing (IBM, Microsoft, etc..)
- API.ai + FB Messenger JS library
- IBM Watson + Slack
- Sentiment analysis can be hooked to the bot

##### Web Speech API
`window.speechSynthesis`
- Client browser linked with Socket.IO => node.js Server => API.ai
- Check out smashing magazine article - Tomomi Imura


### The Long Road to Async / Await in JavaScript
**Thomas Hunter II** @tlhunter

#### Evolution of async control flow
- Phase 1: Callbacks
- Phase 2: Promises
- Phase 3: Generators/Yield
- Phase 4: Async/Await

**Sync code example**
```javascript
let result = sendMessage('tlhunter', 'hi')
console.log(result)

function sendMessage(userId, message) {
  let user = getUser(userId)
  let able = canSend(user)
  if (!able) return false
  return writeMessage(user, message)
}
```

**Sync code with error handling**
```javascript
try {
  var result = sendMessage('tlhunter', 'hi')
} catch (err) {
  return console.error(err)
}
console.log(result)

function sendMessage(userId, message) {
  throw new Error('Bad Stuff')
}
```
All of this is executed step by step. You won't have any execution until the previous function is completed


#### Phase 1: Callbacks
```javascript
sendMessage('tlhunter', 'hi', ⏰(err, result) => {
  console.log(result)
})

function sendMessage(userId, message, cb) {
  getUser(userId, ⏰(err, user) => {
    canSend(user, ⏰(err, able) => {
      if (!able) { cb(null, false); return }
      writeMessage(user, message, cb)
    })
  })
}
```

#### Phase 2: Promises
```javascript
sendMessage('tlhunter', 'hi').then(⏰(result) => {
  console.log(result)
})

function sendMessage(userId, message) {
  return getUser(userId).then(⏰(user) => {
    return canSend(user)
  }).then(⏰(able) => {
    if (!able) return false
    return writeMessage(user, message)
  })
}
```


#### Phase 3: Generators
Not a lot of people use them but they are very powerful
```javascript
let gen = sendMessage('tlhunter', 'hi')
gen.next().value.then(⏰(user) => {
  return gen.next(user).value.then(⏰(able) => {
    return gen.next(able).value.then(⏰(result) => {
      console.log(result)
    })
  })
})
function * sendMessage(userId, message) {
  let user = ⏰yield getUser(userId)
  let able = ⏰yield canSend(user)
  if (!able) return false
  return writeMessage(user, message)
}
```
Interesting back and forth with resolved values. We get a value and pass it back to the generator once resolved.

**Error Handling**
```javascript
const co = require('co')
sendMessage('tlhunter', 'hi').then(⏰(result) => {
  console.log(result)
}).catch(⏰(err) => {
  console.error(err)
})
function sendMessage(userId, message) {
  return co(function * sendMessageGen() {
    ⏰yield Promise.reject(new Error('Bad Stuff'))
  })
}
```

#### Phase 4: Async and Await
```javascript
(async () => {
  let result = ⏰await sendMessage('tlhunter', 'hi')
  console.log(result)
})()

async function sendMessage(userId, message) {
  let user = ⏰await getUser(userId)
  let able = ⏰await canSend(user)
  if (!able) return false
  return writeMessage(user, message)
}
```

**Async/Await Error Handling**
```javascript
(async () => {
  try {
    var result = ⏰await sendMessage('tlh', 'hi')
  } catch(err) {
    return console.error(err)
  }
  console.log(result)
})()

async function sendMessage(userId, message) {
  throw new Error('Bad Stuff')
}
```




Links to slides: https://thomashunter.name/presentations/async-await-javascript-v1/


### Learned lessons from Pinterest rewriting a progressive web app of their native app
Reasons to have a progressive web app:
- Features on desktop don't break mobile
- Less logical complexity
- Additional surface for experimentation
- Optimize for touch surfaces

**Mobile web you want to shoot for 200kb max of JavaScript.**

If you build a progressive web app from scratch
1) Consistency
2) Performance
3) Accessibility

#### Consistency
- Create a component Library
Avoid creating divs in your react code. Create components for each elements
`<Masonry> <Box> <Spinner> <Button>` etc...

