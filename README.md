# discoexpress-require-role

Middleware to require that a user has a role before continuing down a route.

### Example

See [DiscoExpress](https://github.com/tablekat/DiscoExpress) for more.

```js
var DiscoExpress = require('discoexpress');
var app = new DiscoExpress();

app.on("ready", () => console.log("Bot ready"));
app.on("disconnected", () => console.log("Disconnected"));

app.on("message", (bot, msg, next) => {
  // Ignore messages from myself
  // If next is not called, none of the handlers past this point get called!
  if(msg.sender.id != bot.user.id) return next();
});

app.on("message", (bot, msg, next) => {
  // This handler gets called, and then always continues to the next one
  console.log("Got message:", msg.content);
  return next();
});

var requireMod = require('discoexpress-require-role')(["Moderator", "Admin"]);

app.on("message", requireMod, (bot, msg, next) => {
  // Only users with a role named "Moderator" or "Admin" can run this command!
  if(msg.content == "!modcmd"){
    bot.reply(msg, "A moderator ran this command!");
  }else{
    next();
  }
});

app.login("[token]");
```
