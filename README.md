# reactListComponentDelux
This React list App comes replete with alerts, edit, delete and clear all buttons.

This app can easily be cross utilized with ease.

A general overview of the pertinent React code is given below:

To start the states are set up inside of App.js and the start of the return is formatted.
```React
function App() {
  const [name, setName] = useState("");
  const [list, setList] = useState(getLocalStorage());
  const [isEditing, setIsEditing] = useState(false);
  const [editID, setEditID] = useState(null);
  const [alert, setAlert] = useState({ show: false, msg: "", type: "" });

  return (
    <section className="section-center">
      <form className="grocery-form" onSubmit={handleSubmit}>
      </form>
 ```
 
 
 Next the handleSubmit() function was constructed.
 ```React
   const handleSubmit = (e) => {
    e.preventDefault();
    console.log('test')
    }
    ```
    
    Next the alert is displayed by checking the value of show. The alert type depends on that value.
    ```React
     <form className="grocery-form" onSubmit={handleSubmit}>
      {alert.show && <Alert />}
    ```
        
        
  Then go back to formatting the return.
  ```React
  {alert.show && <Alert {...alert} removeAlert={showAlert} list={list} />}

   <h3>🎯 List</h3>
    <div className="form-control">
       <input
         type="text"
          className="grocery"
           placeholder="e.g. get shit done 🤺"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
          <button type="submit" className="submit-btn">
            {isEditing ? "edit" : "submit"}
          </button>
 ```
          
          
 Now that the initial structure is complete functionality can start to be implemented starting with some validations.
 ```React
 const handleSubmit = (e) => {
   e.preventDefault();
    if (!name) {
     //display alert
    } else if (name && isEditing) {
     //deal with edit
    } else {
      showAlert(true, "success", "Got more 💩 to do");
      const newItem = { id: new Date().getTime().toString(), title: name };
      setList([...list, newItem]);
      setName("");
    }
  };
  ```
  
  
  Add a prop to the List component to make the data available in List.js.
  ```React
    <div className="grocery-container">
          <List items={list}  />
  ```
          
          
 Now set up a return inside of List.js
 ```React
 const List = ({ items, removeItem, editItem }) => {
  return (
    <div className="grocery-list">
      {items.map((item) => {
        const { id, title } = item;
        return (
          <article key={id} className="grocery-item">
            <p className="title">{title}</p>
            <div className="btn-container">
              <button
                type="button"
                className="edit-btn"
          
              >
                <FaEdit />
              </button>
              <button
                type="button"
                className="delete-btn"
           
              >
                <FaTrash />
              </button>
            </div>
          </article>
 ```         
  
          
Now the list will display a submitted item. Next conditional rendering is set up in App.js to ensure that buttons and items are only displayed if their are items inside of the div.
```React
</div>
  </form>
      {list.length > 0 && (
        <div className="grocery-container">
          <List items={list} removeItem={removeItem} editItem={editItem} />
          <button className="clear-btn" onClick={clearList}>
            clear items
          </button>
```
          
Now the alerts functionality is fully implemented.
```React
   {alert.show && <Alert {...alert}  />}
          
   const Alert = ({ type, msg }) => {
     return <p className={`alert alert-${type}`}>{msg}</p>;
  
   const [alert, setAlert] = useState({ show: false, msg: "", type: "" });
    
    e.preventDefault();
    if (!name) {
      showAlert(true, "danger", "please enter value");
 
    {alert.show && <Alert {...alert} removeAlert={showAlert} list={list} />}
```
        
        
Deconstruct showAlert inside of Alert.js.
```React
   const Alert = ({ type, msg, removeAlert, list }) => {
```
        
        
Set a timeout function to remove the alerts after 3 seconds.
```React     
  useEffect(() => {
    const timeout = setTimeout(() => {
      removeAlert();
    }, 3000);
    return () => {
      clearTimeout(timeout);
    };
  }, []);
```
  
  
Now implement the clear list functionality.
```React
    } else if (name && isEditing) {
      showAlert(true, "success", "You changing 💩");
    } else {
      showAlert(true, "success", "Got more 💩 to do");
      const newItem = { id: new Date().getTime().toString(), title: name };
            
    const clearList = () => {
    showAlert(true, "danger", "All the 💩 is finished!");
    setList([]);
  };
  
  
   <button className="clear-btn" onClick={clearList}>
       clear items
   </button>
```
          
          
Now the clear all button works, so the individual buttons need to be implemented starting with the delete button.
```React
  const removeItem = (id) => {
    showAlert(true, "danger", "💩 got done!");
    setList(list.filter((item) => item.id !== id));
  };
  
   <button
    type="button"
    className="delete-btn"
    onClick={() => removeItem(id)}
      >
      <FaTrash />
   </button>
```
              
             
Then the edit button.
```React
   const editItem = (id) => {
    const specificItem = list.find((item) => item.id === id);
    setIsEditing(true);
    setEditID(id);
    setName(specificItem.title);
  };
  
     <button
       type="button"
       className="edit-btn"
       onClick={() => editItem(id)}
         >
           <FaEdit />
     </button>
```
              
Finally, render to the page.
```React
  ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```


***End walkthrough
