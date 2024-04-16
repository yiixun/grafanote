# grafanote
A combination of grafana and notebook to build free style internal system


# what can this do
1. with grafana, a quick dashboard system is launched.

2. with jupyter, a notebook is launched.

3. with a demo "apis" server in jupyter, the html/js code embeded in grafana html panel can do api calls!
   then, it is possible to build a customized dashboard with jquery(embeded in grafana), and some simple html/js code!
   every one could create a "component" in a panel, without React/nodejs/vue or any other complex "js framework".

   make front simple again.

# how to
```
# start the env
docker-compose up -d
```

1. open grafana http://localhost/grafana

2. open jupyter http://localhost/py
   open the demo server under work directory.
   start the server by run the notebook.

3. in grafana, create a dashboard, and an HTML panel
   and write some html and js.

```
<script type="text/javascript" language="javascript">
 
// simplify the post requests.
function post(url, payload) {
    var xhttp = new XMLHttpRequest();
    xhttp.open("POST", url, false);
    xhttp.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
    xhttp.send(JSON.stringify(payload)); 
    var response = JSON.parse(xhttp.responseText);
    return response;
}
 
// refresh_list
function todo_refresh() {
    // the api provided by demo server in notebook.
    var api = "http://localhost/apis/todo/refresh";
    var payload = {};
    result = post(api, payload);
    console.log(result);
    // redraw items
    var newHtml = "";
    for (var i = 0; i < result.length; i++) {
        //console.log(key + ": " + result[key]); // name age
        todo = result[i];
        var todoHtml;
        if (todo['status'] == 1) {
            todoHtml = "<div> <input id='todo_" + todo['id']  + "' type='checkbox' checked/> " + todo['name'] +" </div>";
        } else {
            todoHtml = "<div> <input id='todo_" + todo['id']  + "' type='checkbox' > " + todo['name'] +" </div>";
        }
        newHtml += todoHtml;
    }
    document.getElementById("test_div_1").innerHTML = newHtml;
}
 
</script>
 
<div id="todo_div">
click Refresh button to get todo lists.
</div>
 
<button class="sxep88s button" type="submit" onclick="todo_refresh()"> Refresh </button>

```


