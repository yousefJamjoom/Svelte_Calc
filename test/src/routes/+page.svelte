<script>
    let numbers = [1,2,3,4,5,6,7,8,9];
    let operations = ["/","x","+","-"];
    let text = "";
    let width = 50;
    let height = 50;

    function solve(){
        let txt = text;
        let multiply = ["x",(A,B)=>{return A*B}];
        let divide = ["/",(A,B)=>{return A/B}];
        let plus = ["+",(A,B)=>{return A+B}];   
        let minus = ["-",(A,B)=>{return A-B}];
        let operations = [multiply,divide,plus,minus];
        for(let i in operations){
            txt = doOperation(operations[i], txt);
        }
        text = txt;
    }

    function doOperation(op, txt){
        for(let i = 0; i<1000; i++){
            console.log("index = "+i);
            if (i > txt.length){
                break
            }
            if (txt[i] == op[0] && !( i==0 && txt[i] == "-")){
                let list = findAandB(i,txt);
                let A = list[0];
                let B = list[1];
                let indexLeft = list[2];
                let indexRight = list[3];
                console.log("A = "+A);
                console.log("B = "+B);
                console.log("indexLeft = "+indexLeft);
                console.log("indexRight = "+indexRight);
                let Result = op[1](A,B);
                i = indexLeft;
                console.log("Result = "+Result);
                txt = txt.slice(0, indexLeft+1) + Result + txt.slice(indexRight);
                console.log("newText = "+txt);
            }
        }
        return txt;
    }

    function findAandB(i, txt){
        let numA = 0;
        let numB = 0;
        let indexLeft = 0;
        let indexRight = 0;
        for(let pre = i-1; pre >= 0; pre--){
            if (txt[pre] != "x" && txt[pre] != "/" && txt[pre] != "+" && txt[pre] != "-"){
                if(txt[pre] == "."){
                    numA = numA/(10**(""+parseInt(numA)).length)
                } else if(parseInt(numA) != 0){
                    numA += (10**((""+parseInt(numA)).length))*parseFloat(txt[pre]);
                } else {
                    numA += parseFloat(txt[pre]);
                }
                console.log("Value of Num A = "+numA);
            } else {
                indexLeft = pre;
                break;
            }
        }
        let point = 0;
        for(let pre = i+1; pre < txt.length; pre++){
            if (txt[pre] != "x" && txt[pre] != "/" && txt[pre] != "+" && txt[pre] != "-"){
                if (txt[pre] == "."){
                    point = (""+numB).length;
                    continue;
                }
                numB = 10*numB+parseFloat(txt[pre]);
            } else {
                indexRight = pre;
                break;
            }
        }
        if (indexLeft == 0){indexLeft = -1;}
        if (indexRight == 0){indexRight = txt.length;}
        if (point > 0){numB =  numB/(10**(((indexRight-i-2)-point)));}
        return [numA,numB,indexLeft,indexRight];          
    }
</script>


<center>
<table style="">
    <tr><th><h1 style="font-size: 40px;">Calculator</h1></th></tr>
    <tr style="display: block;"><th><div style="border: 1px solid black; width: {(width+4)*4-7}px; height: {height}px; font-size: 45px;">{text}</div></th></tr>
    {#each numbers as num}
        {#if (num-1)%3 == 0}
            <tr style="display: block;">
                {#each numbers.slice(num-1,num+2) as n}
                    <!-- {numbers.slice(num-1,num+2).length} -->
                    <td><button style="width: {width}px; height: {height}px;" on:click={(e)=>{
                        text += e.target.innerText;
                    }}>{n}</button></td>
                {/each}
                <td><button style="width: {width}px; height: {height}px;" on:click={(e)=>{
                    text += e.target.innerText;
                }}>{operations[(num-1)/3]}</button></td>
            </tr>
        {/if}    
    {/each}
    <tr style="display: block;">
        <td><button style="width: {(width+4)*3-4}px; height: {height}px;" on:click={(e)=>{
            text += e.target.innerText;
        }}>0</button></td>
        <td><button style="width: {width}px; height: {height}px;" on:click={(e)=>{
            text += e.target.innerText;
        }}>{operations[3]}</button></td>
    </tr>
    <tr style="display: block;">
        <td><button style="width: {(width+4)*2-4}px; height: {height}px;" on:click={()=>{text=""}}>Reset</button></td>
        <td><button style="width: {(width+4)*2-4}px; height: {height}px;" on:click={solve}>Solve</button></td>
    </tr>
</table>
</center>