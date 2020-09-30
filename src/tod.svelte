<script>
	import { Router, Route, Link } from "svelte-routing";
    import Home from "./Home.svelte";
    import emotion from 'emotion/dist/emotion.umd.min.js';
    const { css } = emotion;
   
  
  let url = "";
	let dat = [];
	let loading = true;
	let inputinfo;
    let newinfo;
    let set;
    let de;
    let o = css`
    display:none;
   
    
    `;
    function close(){
        o = css`
        display:none;
        `;

    }
    function sett(id,des){
        set = id;
        de = des
        console.log(id); 
        o = css`
        
        position:fixed;
        display:block;
        z-index:1;
        width:100%;
        height:100%;
        top: -50px;
        left: 0;
        padding-top: 60px;
        
        background-color:rgba(0, 0, 0, 0.308);
    
        
        
        `;
    }
    

	
const onSubmitForm = async() =>{
	if(inputinfo.length == 0){
        console.log("hello")

    }else{
      try {
        let info  = {
			descript:inputinfo,
		}
        const body = {info};
        const response = await fetch("https://ntodo.herokuapp.com/todos",{
            method:"POST",
            headers:{'Content-Type':"application/json"},
            body: JSON.stringify(body)
        })
        loading =true;
        inputinfo = "";
        
	
	} catch (error) {
		console.log(error.message)
    } 
    try {
         setTimeout( async() => {
           const response = await fetch("https://ntodo.herokuapp.com/todos");
	  const jsonData = await response.json();
	  console.log(jsonData);
	  dat =  jsonData
	  loading = false  
        }, 3000);
     
	} catch (err) {
      console.error(err.message);
	}
}
}
const getTodos = async () => {
    try {
      const response = await fetch("https://ntodo.herokuapp.com/todos");
	  const jsonData = await response.json();
	  console.log(jsonData);
	  dat =  jsonData
	  loading = false
	} catch (err) {
      console.error(err.message);
	}
}
const deleteTodo = async id => {
    try {
      const deleteTodo = await fetch(`https://ntodo.herokuapp.com/todos/${id}`, {
        headers: { 'Content-Type': 'application/json'},
        url: 'https://localhost:44346/Order/Order/GiveOrder',
        method: "DELETE"
      });
      loading = true;

    
    } catch (err) {
      console.error(err.message);
    }
    try {
     
             const response = await fetch("https://ntodo.herokuapp.com/todos");
	  const jsonData = await response.json();
	  console.log(jsonData);
	  dat =  jsonData
	  loading = false
      
     
	} catch (err) {
      console.error(err.message);
	}
  };
  
const updateDescription = async () => {
   
   let oh = {
	 des:newinfo,
	 idous:set
   }
   console.log(JSON.stringify(`${oh}`))
   try {
	
	 const response = await fetch(
	   `https://ntodo.herokuapp.com/todos/${set}`,
	   {
		 method: "PUT",
		 headers: { "Content-Type": "application/json" },
		 body: JSON.stringify(oh)
	   }
	 );

	
   } catch (err) {
	 console.error(err.message);
   }
 };

  getTodos()

</script>
<div>
  
    <div class={o}>
       
        <div class="int" id="animate">
             <button id="cbt"on:click={close} >x</button>
        <button id="ebt"on:click={updateDescription} >go</button>
        <textarea  id="ei" bind:value={newinfo}></textarea>
        </div>
		
    </div>
 
    
	<h1 id="status"></h1>
	
	<div id="inp">
		<h1 id="p">Todo List</h1>
		<textarea id = "text"type="text" bind:value={inputinfo}></textarea>
		<button on:click={onSubmitForm} id="bt">Add</button>
	</div>
	
        {#if loading}
        <div class="loading">
            <div id="p1"></div>
            <div id="p2"></div>
            <div id="p3"></div>
        </div>
     
       
       
      {:else} 
    <div class="cards">
		{#each dat as g(g.todo_id)}
		<div class="card" >
            <h1 id="pd">{g.description}</h1>
            <hr class="ln"> 
            <button id="edt2" on:click={sett(g.todo_id,g.description)} >Edit</button>
            <button
            className="btn btn-danger"
            on:click={() => deleteTodo(g.todo_id)}
            id={g.todo_id,"dbt"}>
                  Delete
                </button>
		  </div>
        {/each}
        </div>
        {/if}
	
</div>

<style>
    #animate {
  -webkit-animation: animatezoom 0.6s;
  animation: animatezoom 0.6s
}
@-webkit-keyframes animatezoom {
  from {-webkit-transform: scale(0)} 
  to {-webkit-transform: scale(1)}
}
  
@keyframes animatezoom {
  from {transform: scale(0)} 
  to {transform: scale(1)}
}

    #ei{
        position: absolute;
        width:70%;
        height: 209px;
        left: 12.9%;
        top: 11%;
        
        background: #FFFDFD;
        color: black;
        
        border-radius: 6px;
        
        border-radius: 6px;
        border-radius: 10px;
            
            font-size: 10px;
            text-align: start;
            -o-text-overflow: clip;
            text-overflow: clip;
            letter-spacing: 3px;
            box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
            border-style: unset;
            outline: none;
            
    }
    hr.ln{
  border: 1px solid rgb(179, 179, 179);
  border-radius: 5px;
}
    .int{
    
        position: absolute;
        width: 70%;
        height:40%;
        left:10%;
        z-index: 3;
        border-radius: 10px;
        top: 30%;
        text-align: center;
		padding: 1em;
		
        margin: 0 auto;
        background-color: rgba(15, 15, 15, 0.788);

    }
   /* #status{
        position:fixed;
        display:block;
        z-index:3;
        width:100%;
        height:100%;
        top: -50px;
        left: 0;
        padding-top: 60px;
        
        background-color:rgba(0, 0, 0, 0.308);
    }*/
    #bt{
        width: 20%;
        height: 20px;
        left: 58.9%;
        top: 30%;
        
        
        
        background: #FFC2C2;
        border-radius: px;
        background-color:rgb(30, 38, 153) ;
           
            
          height: 42px;
          font-family: 'Roboto', sans-serif;
          font-size: 11px;
          letter-spacing: 1.5px;
          font-weight: 650;
          color: white;
          background-color: #244dbd;
          border: none;
          border-radius: 7px;
          box-shadow: 3px 3px 6px rgb(85, 85, 85);
          
          transition: all 0.3s ease 0s;
          cursor: pointer;
          outline: none; 
        
    }
    #ebt{
        position: absolute;
        width: 40%;
        height: 40px;
        left: 28.9%;
        box-shadow: 3px 3px 6px rgb(85, 85, 85);
          
        top: 80%;
        
        
        
        background: #FFC2C2;
       
       
            
         
          font-family: 'Roboto', sans-serif;
          font-size: 11px;
          letter-spacing: 1.5px;
          font-weight: 650;
          color: white;
          background-color: #244dbd;
          border: none;
          border-radius: 7px;
          
          transition: all 0.3s ease 0s;
          cursor: pointer;
          outline: none; 
        
    }
    #cbt{
        position: absolute;
        width: 10%;
        height: 30px;
        left: 89.9%;
        top: 1%;
        
        
        
        background: #FFC2C2;
       
      
        box-shadow: 3px 3px 6px rgb(85, 85, 85);
          
         
          font-family: 'Roboto', sans-serif;
          font-size: 11px;
          letter-spacing: 1.5px;
          font-weight: 650;
          color: white;
          background-color: #25304e79;
          border: none;
          border-radius: 7px;
          box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
          transition: all 0.3s ease 0s;
          cursor: pointer;
          outline: none; 
        
    }
    #dbt{
        box-shadow: 3px 3px 6px rgb(85, 85, 85);
          
        width: 30%;
        height: 20px;
        left: -30.9%;
        top: 100px;
        font-family: 'Roboto', sans-serif;
        font-size: 11px;
        letter-spacing: 1.5px;
        font-weight: 650;
        color: white;
        background-color: #1c263f4b;
        border: none;
        border-radius: 3px;
        
        transition: all 0.3s ease 0s;
        cursor: pointer;
        outline: none; 
}
#edt2{
        position: relative;
        width: 20%;
        height: 20px;
        left: -13%;
        top: 1px;
        font-family: 'Roboto', sans-serif;
        font-size: 11px;
        letter-spacing: 1.5px;
        font-weight: 650;
        color: white;
        background-color: #1c263f4b;
        border: none;
        border-radius: 3px;
        box-shadow: 3px 3px 6px rgb(85, 85, 85);
          
        transition: all 0.3s ease 0s;
        cursor: pointer;
        outline: none; 
}
   #p{
       color: aliceblue;
       text-shadow: 3px 3px 6px rgb(85, 85, 85);
   }
   #pd{
       color: aliceblue;
       text-shadow: 3px 3px 6px rgb(85, 85, 85);
   }
     @keyframes float {
            0% {
                
                transform: translatey(0px);
            }
            50% {
                
                transform: translatey(-20px);
            }
            100% {
                
                transform: translatey(0px);
            }
        }
        
        
    #p1{
        position: absolute;
        left: 45%;
        width: 10px;
        height: 10px;
        background-color: black;
        border-radius: 20px;   
        animation: float 2.3s ease-in-out infinite;
    }
    #p2{
        position: absolute;
        left: 50%;
        width: 10px;
        height: 10px;
        background-color: black;
        border-radius: 20px;   
        animation: float 2.6s ease-in-out infinite;
    }
    #p3{
        position: absolute;
        left: 55%;
        width: 10px;
        height: 10px;
        background-color: black;
        border-radius: 20px;   
        animation: float 2.9s ease-in-out infinite;
    }
	.edt{
		display: none;
	}
#text{
width:100%;
height: 90px;
left: 10.9%;
top: 31%;

background: #242424;
color: rgb(252, 252, 252);

border-radius: 6px;


border-radius: 6px;
border-radius: 10px;
    
    font-size: 15px;
    text-align: start;
    -o-text-overflow: clip;
    text-overflow: clip;
    letter-spacing: 3px;
    box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.1);
    border-color: blue;
   outline: none;
}
.loading{
    text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
}
 .cards {
    max-width:70%;
    height:auto;
  margin: 0 auto;
  display:grid;
  grid-gap: 1rem;
  
  }
.card {
    align-items: center;
    text-align: center;
    border-radius: 20px;
    background-color: rgba(63, 62, 62, 0.91);
    color: black;
    padding: 1rem;
    height:auto;
    width:auto;
    align-items: center;
    box-shadow: 1px 2px 7px rgb(200, 196, 255);
   grid-auto-flow: row;
  }
  .card:hover{
    transform: translatey(-1px);
    opacity: 1.2;
  
    
  }
  @media (min-width: 800px) {
    .cards { 
         grid-auto-flow: row; 
         width:50%
    }
   

  }
  @media (min-width: 900px) {
    .cards { 
         grid-auto-flow: row; 
         width:50%
    }
    #ei{
        width:60%;
        left: 20%;
        height: 209px;
    }
    .int{
        width: 50%;
        height:40%;
        left:20%;
        top: 30%;
    }

  }

  @media (max-width: 400px) {
    .cards {
         grid-auto-flow: row; 

    }
    #pd{
        font-size: 20px;
    }
    #ei{
        width:70%;
        left: 15%;
        height: 100px;
    }
    #ebt{
        width: 40%;
        height: 40px;
        left: 28.9%;
        top: 55%;
    }
    .int{
        width: 60%;
        height:30%;
        left:20%;
        top: 30%;
    }
  }

.g {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	#inp {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
/*
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}*/
   
    .boxes {
    height: 32px;
    width: 32px;
    text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
    -webkit-transform-style: preserve-3d;
    transform-style: preserve-3d;
    -webkit-transform-origin: 50% 50%;
    transform-origin: 50% 50%;
   
    -webkit-transform: rotateX(60deg) rotateZ(45deg) rotateY(0deg) translateZ(0px);
    transform: rotateX(60deg) rotateZ(45deg) rotateY(0deg) translateZ(0px);
}
.boxes .box {
    width: 32px;
    height: 32px;
    top: 0px;
    left: 0;
    position: absolute;
    -webkit-transform-style: preserve-3d;
    transform-style: preserve-3d;
}



.boxes .box:nth-child(1) {
    -webkit-transform: translate(100%, 0);
    transform: translate(100%, 0);
    -webkit-animation: box1 1s linear infinite;
    animation: box1 1s linear infinite;
}
.boxes .box:nth-child(2) {
    -webkit-transform: translate(0, 100%);
    transform: translate(0, 100%);
    -webkit-animation: box2 1s linear infinite;
    animation: box2 1s linear infinite;
}
.boxes .box:nth-child(3) {
    -webkit-transform: translate(100%, 100%);
    transform: translate(100%, 100%);
    -webkit-animation: box3 1s linear infinite;
    animation: box3 1s linear infinite;
}
.boxes .box:nth-child(4) {
    -webkit-transform: translate(200%, 0);
    transform: translate(200%, 0);
    -webkit-animation: box4 1s linear infinite;
    animation: box4 1s linear infinite;
}



.boxes .box > div {
    background: #5C8DF6;
    --translateZ: 15.5px;
    --rotateY: 0deg;
    --rotateX: 0deg;
    position: absolute;
    width: 100%;
    height: 100%;
    background: #5C8DF6;
    top: auto;
    right: auto;
    bottom: auto;
    left: auto;
    -webkit-transform: rotateY(var(--rotateY)) rotateX(var(--rotateX)) translateZ(var(--translateZ));
    transform: rotateY(var(--rotateY)) rotateX(var(--rotateX)) translateZ(var(--translateZ));
}

.boxes .box > div:nth-child(1) {
    top: 0;
    left: 0;
    background: #5C8DF6;
}
.boxes .box > div:nth-child(2) {
    background: #145af2;
    right: 0;
    --rotateY: 90deg;
}
.boxes .box > div:nth-child(3) {
    background: #447cf5;
    --rotateX: -90deg;
}
.boxes .box > div:nth-child(4) {
    background: #DBE3F4;
    top: 0;
    left: 0;
    --translateZ: -90px;
}





@keyframes box1 {
    0%,
    50% {
        transform: translate(100%, 0);
    }
    100% {
        transform: translate(200%, 0);
    }
}

@keyframes box2 {
    0%{
        transform: translate(0, 100%);
    }
    50% {
        transform: translate(0, 0);
    }
    100% {
        transform: translate(100%, 0);
    }
}

@keyframes box3 {
    0%,
    50% {
        transform: translate(100%, 100%);
    }
    100% {
        transform: translate(0, 100%);
    }
}

@keyframes box4 {
    0%{
        transform: translate(200%, 0);
    }
    50% {
        transform: translate(200%, 100%);
    }
    100% {
        transform: translate(100%, 100%);
    }
}
</style>