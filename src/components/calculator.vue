<template>
	<div class="calculator-container">
		<input type="text" @focus="focus" @blur="loseFocus" @keyup="checkCharacter"  v-model="currentNumber" class="currentNumber">
		<div>
			<button @click="clear">{{clearText}}</button>
			<button @click="back">&times;</button>
			<button @click="percent">%</button>
			<button @click="numberCome('/')" class="operator">/</button>
		</div>
		<div>
			<button @click="numberCome('7')">7</button>
			<button @click="numberCome('8')">8</button>
			<button @click="numberCome('9')">9</button>
			<button @click="numberCome('x')" class="operator">X</button>
		</div>

		<div>
			<button @click="numberCome('4')">4</button>
			<button @click="numberCome('5')">5</button>
			<button @click="numberCome('6')">6</button>
			<button @click="numberCome('-')" class="operator">-</button>
		</div>
		<div>
			<button @click="numberCome('1')">1</button>
			<button @click="numberCome('2')">2</button>
			<button @click="numberCome('3')">3</button>
			<button @click="numberCome('+')" class="operator">+</button>
		</div>
		<div>
			<button @click="numberCome('0')" class="zero">0</button>
			
			<button @click="numberCome('.')">.</button>
			<button @click="equal" class="operator">=</button>
		</div>

	</div>
</template>


<script>
	export default {
		name:"calculator",
		data(){
			return {
				currentNumber:'0',
				prevNumber:'',
				fromResult:false,
				signs:['+','-','/','x','*'],
				copy:[]
			}
		},

		methods:{
			
			clear(){
				this.currentNumber='0';
				this.sign=''
				this.prevNumber=''
			},
			back(){
				let c=this.currentNumber;
				this.currentNumber=c.slice(0,c.length-1);
			},
			percent(){
				this.equal();
             this.currentNumber
             let d=parseFloat(this.currentNumber)/100;
             this.currentNumber=d.toString();

			},

			numberCome(number){

                let c=this.currentNumber;
				if(c=='0'){this.currentNumber=number}else{this.currentNumber= this.currentNumber+number}
					this.validate(this.currentNumber);
				


				},

			checkCharacter(e){
				let c=this.currentNumber;
			this.currentNumber=c.replace(/[^x+-/*0123456789]/ig,'');
			if(e.keyCode==13){this.equal();return false}
			
			this.validate(c);

			},
			focus(){
              if(this.currentNumber=="0"){this.currentNumber=''}
			},
		loseFocus(){
              if(this.currentNumber==""){this.currentNumber='0';this.clearText='AC'}
			},

			validate(c){
			let getArray=c.split('');
			let characters=[];
			let digits=[];
			let digit='';
			let indexAlready='no index';

			for(let i=0;i<getArray.length;i++){
			if(getArray[i].search(/[0-9]/) !=-1){
				if(getArray[i]==='0' && i+1<getArray.length){
					getArray[i+1] !='.' && digit =='' ?'':digit +=getArray[i]
				}else{digit +=getArray[i]}
			

			}else{
			if(getArray[i]=='.'){
			if(digit.indexOf('.') !=-1){indexAlready=digits.length;}else{digit +='.'}
			}else{
			if(digit !='' ){digits.push(digit); digit=''}
			characters.push(getArray[i]);
			}

			}
			}

			if(digit !=""){digits.push(digit)}

			if(digits.length<characters.length){
			this.currentNumber=c.slice(0,c.length-1)
			}

			let results='';
			for(let d=0;d<digits.length;d++){
			if(d==indexAlready){results +=this.copy[d]}else{results +=digits[d]}
			if(d<characters.length){results +=characters[d]}

			}
			this.copy=digits;
			this.currentNumber=results;

			},


			equal(){
				this.currentNumber=="" ? this.currentNumber=0:''
				let c=this.currentNumber;
				let lastCharcter=c.slice(-1);
				let indexLast=this.signs.indexOf(lastCharcter);
				if(indexLast !=-1 || lastCharcter =='.'){this.currentNumber=c.slice(0,c.length-1)}

				let finalResult=0;
				let firstArray=this.currentNumber.split('+').toString().split('-');
				for(let i=0;i<firstArray.length;i++){
				i ==0 ? "" :firstArray[i]='-'+firstArray[i];
				}
				let final=firstArray.toString().split(',');

				let nextResult=final.map(block=>{
				let newblockX=block.split('x');
				let newblockM=block.split('*');
				let newblockD=block.split('/');
				let myResult1=0;
				let multiple='';
				if(newblockM.length>1 || newblockX.length>1){

				multiple=newblockM.length>1 ? newblockM : newblockX;
				myResult1= this.RunMultiples(multiple);
				}else if(newblockD.length>1){
				myResult1= this.RunDivisions(newblockD)
				}else{
					myResult1=block;
				}
				return myResult1;
				});
				for(let i=0;i<nextResult.length;i++){finalResult=finalResult+parseFloat(nextResult[i])}
				this.fromResult=true;this.currentNumber=finalResult.toString();

				
  
			},
			RunMultiples(multiple){
				let final=0;
				let results= this.RunDivision(multiple);
				for(let i=0;i<results.length;i++){
				i==0 ? final=parseFloat(results[i]):final=final * parseFloat(results[i]);
				}
				return final;
			},
			RunDivisions(division){
				let final=0;
				let results= division;
				for(let i=0;i<results.length;i++){
				i==0 ? final=parseFloat(results[i]):final=final / parseFloat(results[i]);
				}
				return final;
			},

			RunDivision(devision){
				let results= devision.map(item=>{
				let newItem=item.split('/');
				if(newItem.length>1){
				let prevResult=0;
				for(let i=0;i<newItem.length;i++){
				i==0 ? prevResult=parseFloat(newItem[i]):prevResult=prevResult / parseFloat(newItem[i]);
				}
				return prevResult;

				}else{
				return newItem;
				}

				});
				return results;
			},
		},//method end here
		computed:{
		clearText:function(){
		let text;
		this.currentNumber !='0' ? text="C" :text="AC";
		return text;
		}
		}
	}
</script>

<style scoped>
	
	div.calculator-container{
		background-color: #eee;
	}
		input.currentNumber{
		padding: 15px 0;
		font-size: 30px;
		box-sizing: border-box;
		background: #4949a2;
		color: #fff;
		width: 100%;
		text-indent: 5px;

		}
	button{
		padding:10px 0px;
		font-size: 30px;
		width: 25%;
		cursor: pointer;
	}
	button.operator{
		background-color: orange;
		border-color:#ffa5005c;
		color:white;
	}

	button.zero{
		width:50%;
	}
</style>