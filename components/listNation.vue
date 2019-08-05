<template>
    <div>
        <h1>List Nation</h1>
        <div class="row" style="margin-bottom:10px;">
        	<span
        		:class="region == 'All' ? btnRegionActive : btnRegion"
        		@click="region = 'All'; search = ''"
        	>
        		All
        	</span>
        	<span
        		:class="region == 'Africa' ? btnRegionActive : btnRegion" style="margin-left:5px;"
        		@click="region = 'Africa'; search = ''"
        	>
        		Africa
        	</span>
        	<span
        		:class="region == 'Americas' ? btnRegionActive : btnRegion" style="margin-left:5px;"
        		@click="region = 'Americas'; search = ''"
        	>
        		America
        	</span>
        	<span
        		:class="region == 'Asia' ? btnRegionActive : btnRegion" style="margin-left:5px;"
        		@click="region = 'Asia'; search = ''"
        	>
        		Asia
        	</span>
        	<span
        		:class="region == 'Europe' ? btnRegionActive : btnRegion" style="margin-left:5px;"
        		@click="region = 'Europe'; search = ''"
        	>
        		Europe
        	</span>
        	<span
        		:class="region == 'Oceania' ? btnRegionActive : btnRegion" style="margin-left:5px;"
        		@click="region = 'Oceania'; search = ''"
        	>
        		Oceania
        	</span>
        </div>
    	<div class="row" style="margin-bottom:10px;">
        	<span style="width: 38%;">
        		<div class="input-group">
        		  	<div class="input-group-prepend">
        		    	<span class="input-group-text" id="basic-addon1"> Search </span>
        		  	</div>
        		  	<input 
        		  		type="text" 
        		  		class="form-control" 
        		  		placeholder="Nation Name"
        		  		v-model="search"
        		  		@keyup="getListNation(); region = 'All'"
        		  	>
        		</div>
        	</span>
    	</div>
        <div class="row">

        	<div :class="leftContent">

        		<div class="row">
		        	<div
		        		style="width:25%; margin-right:5px; margin-bottom:5px;"
		        		v-for="res in result"
		        		:key="res.alpha2Code"
		        	>
				        <div class="card">
				        	<img :src="res.flag" class="card-img-top" :alt="res.name">
				          	<div class="card-body">
				            	<h5 class="card-title">{{ res.name }}</h5>
					            <span class="btn btn-primary" @click="getDetail(res.name)">Detail</span>
				          	</div>
				        </div>
		        	</div>
        		</div>
	        	
        	</div>
        	<div class="col-md-4 col-sm-12" v-if="detail == true">
        		<img :src="resultDetail.flag" class="card-img-top" :alt="resultDetail.name">
        		<div>
        			<ul>
        				<li>
        					<h2>{{ resultDetail.name }}</h2>
        				</li>
        				<li>
        					Capital : {{ resultDetail.capital }}
        				</li>
        				<li>
        					Region : {{ resultDetail.region }}
        				</li>
        				<li v-for="dom in resultDetail.topLevelDomain" :key="dom">
        					Top Level Domain : {{ dom }}
        				</li>
        				<li v-for="time in resultDetail.timezones" :key="time">
        					Time Zone : {{ time }}
        				</li>
        				<li>
        					Population : {{ resultDetail.population }}
        				</li>
        				<li>
        					Language : <ol>
        									<li v-for="lang in resultDetail.languages" :key="lang.name">
        										Name : {{ lang.name }}
        									</li>
        									<li v-for="lang in resultDetail.languages" :key="lang.name">
        										Native Name : {{ lang.nativeName }}
        									</li>
        								</ol>
        				</li>
        				<li v-for="code in resultDetail.callingCodes" :key="code">
        					Calling Codes : {{ code }}
        				</li>
        			</ul>
        		</div>
        	</div>
	        
        </div>

    </div>
</template>
<script>

	import axios from 'axios'
    export default {
        data: function() {
            return {
                param   		: {},
                result  		: [],
                resultDetail  	: {},
                detail  		: false,
                search  		: '',
                region			: 'All',
                btnRegion		: 'btn btn-outline-secondary',
                btnRegionActive	: 'btn btn-secondary',
                leftContent		: 'col-md-12 col-sm-12',
            }
        },
        mounted() {
            this.getListNation()
        },
        watch: {
        	region: function() {
        		this.getListNation()
        	}
        },
        methods: { 
        	getListNation() {

        		let url = this.region != 'All' ? `https://restcountries.eu/rest/v2/region/${this.region}` : `https://restcountries.eu/rest/v2/all` 
        		if (this.search != '') {
        			url = `https://restcountries.eu/rest/v2/name/${this.search}`
        		}

        		axios.get(url)
        		    .then((response) => 
        		    {
    		            this.result = response.data
        		    })
        		    .catch((response) => 
        		    {
        		        console.log(response)
        		    })
        	},

        	getDetail(name) {
        		this.detail = true
        		this.leftContent = 'col-md-8 col-sm-12'

        		let url = `https://restcountries.eu/rest/v2/name/${name}?fullText=true`

	    		axios.get(url)
	    		    .then((response) => 
	    		    {
	    		    	// console.log(response.data[0])
			            this.resultDetail = response.data[0]
	    		    })
	    		    .catch((response) => 
	    		    {
	    		        console.log(response)
	    		    })
        	}
        }
    }

</script>
<style>
</style>
