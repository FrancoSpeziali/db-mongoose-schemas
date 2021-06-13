# Snippets

Small pieces of code to help you with this assignment

##### UserSchema template
```javascript
const UserSchema = new mongoose.Schema({
	firstName: { type: String, required: true }
});
```

##### User Model template

```javascript
const User = mongoose.model('User', UserSchema);
```