<template>
  <v-app v-cloak>
    
    <v-app-bar
      	color="black"
     	 dark
    >
      	<v-toolbar-title>Metrics Builder</v-toolbar-title>
      	<v-spacer></v-spacer>
      	<small>{{username}}</small>
    </v-app-bar>

    <v-content class="grey lighten-5">
		
		<v-container  >

			<v-row>
				
			<!--=====================================================================-->
    		<!--BUILDER-->
			<!--=====================================================================-->
					
				<v-col cols="4">
					<v-card class="grey lighten-3">
						<v-tabs background-color="indigo" dark color="white" >
						
							<v-tab>General</v-tab>
							<v-tab>Dataset</v-tab>
							<v-tab>Metric</v-tab>
							<v-tab>Goal</v-tab>

							<!--General Tab-->
							<v-tab-item>
								<v-card flat class="red lighten-3" :height="tabcardheight" style="overflow-y: scroll; overflow-x: hidden;">
									<v-row>
									
										<!--Field: METRIC TITLE-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item two-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">TITLE</v-list-item-title>
														<v-list-item-subtitle>Enter a short, but descriptive title for this metric</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-text-field
													label="Title"
													placeholder="Your Answer"
													outlined
													class="mx-4"
													v-model="title"
												></v-text-field>
											</v-card>
										</v-col>
											
										<!--Field: METRIC SUBTITLE-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item two-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">SUB-TITLE</v-list-item-title>
														<v-list-item-subtitle>Enter a sub-title (optional)</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-text-field
													label="Title"
													placeholder="Your Answer"
													outlined
													class="mx-4"
													v-model="subtitle"
												></v-text-field>
											</v-card>
										</v-col>
										
										<!--Field: METRIC DESCRIPTION-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item two-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">DESCRIPTION</v-list-item-title>
														<v-list-item-subtitle>Enter a short, but descriptive title for this metric</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
													<v-textarea
													label="Description"
													placeholder="Your Answer"
													outlined
													class="mx-4"
													v-model="description"
													></v-textarea>
											</v-card>
										</v-col>
											
										<!--Field: METRIC OR STAT-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">METRIC OR STAT?</v-list-item-title>
														<v-list-item-subtitle>Is this going to be a metric (it will gave a goal) or a stat (work-load indicator)?</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-radio-group v-model="metricorstat" :mandatory="false" class="mx-4">
													<v-radio label="Metric" value="metric"></v-radio>
													<v-radio label="Stat" value="stat"></v-radio>
												</v-radio-group>
											</v-card>
										</v-col>
										
										<!--Field: PRODUCTION STATUS-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">PRODUCTION STATUS</v-list-item-title>
														<v-list-item-subtitle>When "In Production" is selected, the metric will be visible to general users in the front-end app. When "In Development" is selected, the metric is only visible to users who log-in to the front-end app.</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-radio-group v-model="productionstatus" :mandatory="false" class="mx-4">
													<v-radio label="In Production" value="inproduction"></v-radio>
													<v-radio label="In Development" value="indevelopment"></v-radio>
												</v-radio-group>
											</v-card>
										</v-col>
																					
									</v-row>
								</v-card>
							</v-tab-item>
								
							<!--Dataset Tab-->
							<v-tab-item>
								<v-card flat class="red lighten-3" :height="tabcardheight" style="overflow-y: scroll; overflow-x: hidden;">							
									<v-row>
										
										<!--Field: STORED PROCEDURE-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">STORED PROCEDURE</v-list-item-title>
														<v-list-item-subtitle>Select your stored procedure, then click "Continue" to see a sample of its data.</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-select
													:items="storedprocedures"
													v-model="storedprocedure"
													label="Select"
													class="mx-4"
												></v-select>
												
												<v-divider></v-divider>

												<v-card-actions>
													<v-btn 
														color="primary" 
														v-on:click="getStoredProcedureSchema(storedprocedure)"
														class="mx-4">
														Get Data
													</v-btn>
												</v-card-actions>
												
											</v-card>
										</v-col>
											
										<!--Field: VALUE-FIELD-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">VALUE FIELD</v-list-item-title>
														<v-list-item-subtitle>Select the field that will be calculated</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-select
													:items="schemaFieldList"
													v-model="valuefield"
													label="Select"
													class="mx-4"
												></v-select>
												
											</v-card>
										</v-col>
										
										<!--Field: DATE-FIELD-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">DATE FIELD</v-list-item-title>
														<v-list-item-subtitle>Select the date-field that will be used in the line-chart</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-select
													:items="schemaFieldList"
													v-model="datefield"
													label="Select"
													class="mx-4"
												></v-select>
												
											</v-card>
										</v-col>
                    
									</v-row>
								</v-card>
							</v-tab-item>
									
							<!--Metric Tab-->
							<v-tab-item>
								<v-card flat class="red lighten-3" :height="tabcardheight" style="overflow-y: scroll; overflow-x: hidden;">										
									<v-row>
									
										<!--Field: AGGREGATE TYPE-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">AGGREGATE TYPE</v-list-item-title>
														<v-list-item-subtitle>How do you want this field aggregated?</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-select
													:items="aggregatetypes"
													v-model="aggregatetype"
													label="Select"
													class="mx-4"
												></v-select>
												
											</v-card>
										</v-col>
										
										<!--Field: FORMAT-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">FORMAT</v-list-item-title>
														<v-list-item-subtitle>Use standard formatting syntax. Examples: #,### = 1,234. Your value-field will be formatted server-side</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-text-field
													label="Format"
													placeholder="Your Answer"
													outlined
													class="mx-4"
													v-model="valuefieldformat"
												></v-text-field>
											</v-card>
										</v-col>

										<!--Field: VALUE-TYPE-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">VALUE-TYPE</v-list-item-title>
														<v-list-item-subtitle>Minutes, Dollars, Miles, Donuts?</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-text-field
													label="Format"
													placeholder="Your Answer"
													outlined
													class="mx-4"
													v-model="valuetype"
												></v-text-field>
											</v-card>
										</v-col>
											
									</v-row>
								</v-card>
							</v-tab-item>
								
							<!--Goal Tab-->
							<v-tab-item>
								<v-card flat class="red lighten-3" :height="tabcardheight" style="overflow-y: scroll; overflow-x: hidden;">										
									<v-row>
									
										<!--Field: GOAL OPERATOR-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item two-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">GOAL</v-list-item-title>
														<v-list-item-subtitle>If your goal is numeric, choose GT, LT or EQ.</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												
												<v-row>
													<v-col cols="5">
														<v-select
															:items="goaloperatortypes_array"
															v-model="goaloperatortypedisplay"
															label="Select"
															class="mx-4"
														></v-select>
													</v-col>
													
													<v-col cols="7">
														<v-text-field
															label="Goal (integer or decimal)"
															placeholder="Your Answer"
															outlined
															class="mx-4"
															:rules="goalvaluerules"
															v-model="goalvalue"
														></v-text-field>
													</v-col>
												</v-row>													
											</v-card>
										</v-col>
																						
										<!--Field: GOAL WIGGLE ROOM-->
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item three-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">GOAL TOLERANCE</v-list-item-title>
														<v-list-item-subtitle>
															Percent by which the metric can exceed the goal and be in the <b class="yellow black-text">yellow</b>-range
														</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												<v-text-field
													label="Decimal Percent (ie 50% = 0.5)"
													placeholder="Your Answer"
													outlined
													class="mx-4"
													:rules="goalwiggleroomrules"
													v-model="goalwiggleroom"
												></v-text-field>
											</v-card>
										</v-col>
											
										<!--Field: GOAL COLOR PREVIEW-->
										<!--
										<v-col cols="12">
											<v-card class="mx-auto" max-width="500">
												<v-list-item two-line>
													<v-list-item-content>
														<v-list-item-title class="subtitle-1">GOAL COLOR RANGE</v-list-item-title>
														<v-list-item-subtitle>Preview of your goal and tolerance</v-list-item-subtitle>
													</v-list-item-content>
												</v-list-item>
												
												<v-card flat class="mx-4" >
													<v-card
														class="d-flex flex-row mb-6"
														flat
														tile
													>
														<v-card 
															:width="500/3" flat outlined tile class="pa-2 body-2 text-center font-weight-bold" 
															:class="{'green': goaloperatortype=='<', 'red lighten-2': goaloperatortype=='>'}">
															{{(goaloperatortype == "LT" ? color_ranges.green.display : color_ranges.red.display)}}
														</v-card>
														
														<v-card 
															:width="500/3" flat outlined tile class="pa-2 yellow body-2 text-center font-weight-bold">
															{{color_ranges.yellow.display}}
														</v-card>
														
														<v-card 
															:width="500/3" flat outlined tile class="pa-2 body-2 text-center font-weight-bold" 
															:class="{'green': goaloperatortype=='>', 'red lighten-2': goaloperatortype=='<'}">
															{{goaloperatortype == '>' ? color_ranges.green.display : color_ranges.red.display}}
														</v-card>
													</v-card>
													
													<v-card-actions>
														
														<v-spacer></v-spacer>

														<v-btn
															text
															@click="showGoalColorTable = !showGoalColorTable"
														>
															<v-icon>{{ showGoalColorTable ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon> 
															Table
														</v-btn>
													</v-card-actions>
														
													<v-expand-transition>
														<div v-show="showGoalColorTable">

															<v-simple-table>
																<template v-slot:default>
																	<thead>
																		<tr>
																			<th class="text-left" style="width: 200px;"></th>
																			<th class="text-right">Start</th>
																			<th class="text-right">End</th>
																		</tr>
																	</thead>
																	<tbody>
																		<tr class="">
																			<td>Green</td>
																			<td class="text-right">{{color_ranges.green.start == -1 ? '∞' : color_ranges.green.start }}</td>
																			<td class="text-right">{{color_ranges.green.end == -1 ? '∞' : color_ranges.green.end }}</td>
																		</tr>
																		<tr class="">
																			<td>Yellow</td>
																			<td class="text-right">{{color_ranges.yellow.start}}</td>
																			<td class="text-right">{{color_ranges.yellow.end}}</td>
																		</tr>
																		<tr class="">
																			<td>Red</td>
																			<td class="text-right">{{color_ranges.red.start == -1 ? '∞' : color_ranges.red.start }}</td>
																			<td class="text-right">{{color_ranges.red.end == -1 ? '∞' : color_ranges.red.end }}</td>
																		</tr>
																	</tbody>
																</template>
															</v-simple-table>
														</div>
													</v-expand-transition>
														
												</v-card>
												
												<v-divider></v-divider>
											</v-card>
										</v-col>
										-->
									</v-row>										
								</v-card>
							</v-tab-item>
							
						</v-tabs> <!--End Tabs-->
					</v-card>
				</v-col>
			

				<!--=====================================================================-->
        		<!--PREVIEWS-->
				<!--=====================================================================-->
			
				<v-col cols="8">
					<v-row >
						<v-col cols="12" style="padding-top:0;">
							<v-card class="grey lighten-3 mx-auto">
							
								<v-tabs background-color="blue" color="white" >
				
									<v-tab>Card</v-tab>
									<v-tab>Raw Data</v-tab>
									<v-tab>Schema</v-tab>

									<!--Card Tab-->
									<v-tab-item class="grey lighten-3" >
									
										<v-card flat class="grey lighten-3 mx-8 my-8" height="745">
											<v-container fill-height fluid>
											<v-row align="center" justify="center">
											
												<v-col cols="4">
													<v-card class="mx-auto"	max-width="400">

														<!--Title and Subtitle-->
														<v-list-item two-line>
															<v-list-item-content>
															<v-list-item-title class="headline">{{title || "Enter a title"}}</v-list-item-title>
															<v-list-item-subtitle>{{subtitle}}</v-list-item-subtitle>
															</v-list-item-content>
														</v-list-item>
										
														<v-divider></v-divider>
														
														<v-card-text>
															<v-row>

																<!--Current Value-->
																<v-col class="" style="height:75; border-right: solid 1px gainsboro" cols="8" align="center">

																	<v-card-text class="display-3 pa-0" style="line-height:2rem; height:100%;">
																		{{_format(valuefieldformat,preview_value)}}
																	</v-card-text>
																	
																	<v-card-text class="overline mx-auto mt-n6 pb-0" align="center">{{valuetype}}</v-card-text>

																</v-col>

																<!--Current Color-->
																<v-col cols="4" class="pt-0" style="height:75px;" align="center">

																	<v-card-text class="pa-0 pt-3" v-if="metricorstat == 'metric'">
																		<v-icon large color="green" v-if="preview_goalcolor == 'green'">lens</v-icon>
																		<v-icon medium color="grey" v-if="preview_goalcolor != 'green'">lens</v-icon>
																		
																		<v-icon large color="yellow" v-if="preview_goalcolor == 'yellow'">lens</v-icon>
																		<v-icon medium color="grey" v-if="preview_goalcolor != 'yellow'">lens</v-icon>
																		
																		<v-icon large color="red" v-if="preview_goalcolor == 'red'">lens</v-icon>
																		<v-icon medium color="grey" v-if="preview_goalcolor != 'red'">lens</v-icon>
																	</v-card-text>

																	<v-card-text class="pa-0 pt-4" v-if="metricorstat == 'stat'">
																		Statistic Only No Goal
																	</v-card-text>
																	
																</v-col>

															</v-row>

															<v-row>
																<v-col cols="8" class="pa-0 pt-2">
																	<v-card-text class="overline mx-auto pa-0 font-weight-black" align="center">
																		Latest Value
																	</v-card-text>
																</v-col>
																<v-col cols="4" class="pa-0 pt-2">
																	<v-card-text class="overline mx-auto pa-0 font-weight-black" align="center">
																		Status
																	</v-card-text>
																</v-col>
															</v-row>
														</v-card-text>
														
														<v-divider></v-divider>

														<!--Card Footer-->
														<v-card-actions>
												
															<v-spacer></v-spacer>

															<v-btn text	@click="showMetricDescription = !showMetricDescription">
																<v-icon>{{ showMetricDescription ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon> 
																Details
															</v-btn>

														</v-card-actions>
											
														<v-expand-transition>
															<div v-show="showMetricDescription">

															<!--Description-->
															<v-card-text>
																<v-list-item-title class="subtitle-1 mb-1">Metric Description</v-list-item-title>
																{{description}}
															</v-card-text>


															<!--GOAL RANGE-->
															<v-card-text>

																<v-list-item-title class="subtitle-1 mb-1">Goal Range</v-list-item-title>

																<v-card flat class="mx-4" >
																	<v-card class="d-flex flex-row mb-0" flat	tile>
																		<v-card 
																			:width="500/3" flat outlined tile class="pa-1 caption text-center font-weight-bold" 
																			:class="goaloperatortypeabbr == 'lt' ? 'green' : 'red'">
																			{{(goaloperatortypeabbr == "lt" ? color_ranges.green.display : color_ranges.red.display)}}
																		</v-card>
																		
																		<v-card 
																			:width="500/3" flat outlined tile class="pa-1 yellow caption text-center font-weight-bold">
																			{{color_ranges.yellow.display}}
																		</v-card>
																		
																		<v-card 
																			:width="500/3" flat outlined tile class="pa-1 caption text-center font-weight-bold" 
																			:class="goaloperatortypeabbr == 'lt' ? 'red' : 'green'">
																			{{goaloperatortypeabbr == 'gt' ? color_ranges.green.display : color_ranges.red.display}}
																		</v-card>
																	</v-card>
													
																</v-card>

																<v-card flat class="mx-4">
																	
																	<v-card	class="d-flex flex-row mb-1" flat tile>
																		<v-card 
																			:width="500/3" flat tile class="pa-0 overline text-center">
																			{{(goaloperatortypeabbr == "lt" ? "Green" : "Red")}}
																		</v-card>
																		
																		<v-card 
																			:width="500/3" flat tile class="pa-0 overline text-center">
																			Yellow
																		</v-card>
																		
																		<v-card 
																			:width="500/3" flat tile class="pa-0 overline text-center">
																			{{goaloperatortypeabbr == 'gt' ? "Green" : "Red"}}
																		</v-card>
																	</v-card>
													
																</v-card>

															</v-card-text>

															</div>
														</v-expand-transition>
													</v-card>
												</v-col>
							
												<v-col cols="8">
									
													<apexchart 
														type="line" 
														height="300" 
														:options="chartOptions" 
														:series="series">
													</apexchart>
									
												</v-col>

											</v-row>
											</v-container>
										</v-card>
                  </v-tab-item>

									<!--Raw Data Tab-->
									<v-tab-item class="grey lighten-3">
										<v-row>
											
											<v-col cols="12">
												<v-card flat class="grey lighten-3" height="745" style="overflow-y:scroll; overflow-x:hidden;">
										
													<v-card-text>
													
														<v-data-table 
															fixed-headers 
															:headers="storedprocedureheaders"
															:items="storedproceduredataset"
															:items-per-page="15"
															class="elevation-1"
														></v-data-table>													

													</v-card-text>
												</v-card>
											</v-col>
										</v-row>
									</v-tab-item>
										
									<!--Schema-->
									<v-tab-item>
										<v-row>											
											<v-col cols="12">
												<v-card flat class="grey lighten-3" height="745" style="overflow-y:scroll; overflow-x:hidden;">
										
													<v-card-text>
													
														<v-simple-table style="width:50%;">
															<template v-slot:default>
															<thead>
																<tr>
																<th class="text-left">Field</th>
																<th class="text-left">SQL Type</th>
																<th class="text-left">JS Type</th>
																</tr>
															</thead>
															<tbody>
																<tr v-for="field in storedprocedureschema" :key="field.fieldname">
																<td>{{ field.fieldname }}</td>
																<td>{{ field.sqltype }}</td>
																<td>{{ field.jstype }}</td>
																</tr>
															</tbody>
															</template>
														</v-simple-table>													

													</v-card-text>
												</v-card>
											</v-col>
										</v-row>
									</v-tab-item>

								</v-tabs>
							</v-card>
						</v-col>
					</v-row>	
				</v-col>	
			</v-row>

			<v-row>
				<v-col cols="12" class="text-right">
					<v-btn color="primary">Continue</v-btn>
				</v-col>
			</v-row>
		</v-container>
	</v-content>
</v-app>
</template>

<script>
import Vue from 'vue'
import VueApexCharts from 'vue-apexcharts'
import format from 'number-format.js'
import VueMoment from 'vue-moment'
import moment from 'moment'
 
Vue.use(VueMoment, {
    moment,
})

export default {
  name: 'App',

  components: {
    	apexchart: VueApexCharts,
  },

  data: () => ({  
		title: '',
		subtitle: '',
		description: '',
		metricorstat: 'metric',
		productionstatus: 'indevelopment',
		goalcolor: 'red',
		goaloperatortypedisplay: '',
		goaloperatortypes: [
			{name: 'Equals', symbol: '=', abbr: "eq"},
			{name: 'Greater Than', symbol: '>', abbr: "gt"},
			{name: 'Less Than',symbol: '<', abbr: "lt"},
			{name: 'Increase', symbol: '+', abbr: "in"},
			{name: 'Decrease', symbol: '-', abbr: "de"}
		],
		
		goalvalue: 0,
		goalwiggleroom: 0.2, //always a percent
		
		goalvaluerules: [
			value => !isNaN(parseFloat(value)) && isFinite(value) || 'Numeric values only',
		],
		goalwiggleroomrules: [
			value => !isNaN(parseFloat(value)) && isFinite(value) || 'Numeric values only',
			value => value < 1,
		],
									
		valuefield: 'Response_Time',
		datefield: 'Date_Received',
		dategroupby: 'day',
		valuefieldformat: '#,##0.00',
		valuetype: "minutes",
		aggregatetype: 'Average',
		aggregatetypes: ['Average','Sum','Count','Max','Min','Latest Record'],
		storedprocedures: [],
		storedprocedure: "",
		storedproceduredataset: [],
		storedprocedureschema: [],
		storedprocedureschemaselected: [],
		storedprocedureheaders: [],
		
		tabcardheight: 770,
		showMetricDescription: false,
		showGoalColorTable: false,
		
		username: "",
		auth_token: "",	

		datatypes: [
			{sql: "bigint", js: "number"},
			{sql: "smallint", js: "number"},
			{sql: "tinyint", js: "number"},
			{sql: "int", js: "number"},
			{sql: "decimal", js: "number"},          
			{sql: "numeric", js: "number"},
			{sql: "money", js: "number"},
			{sql: "smallmoney", js: "number"},
			{sql: "float", js: "number"},
			{sql: "real", js: "number"},

			{sql: "bit", js: "boolean"},

			{sql: "date", js: "date"},
			{sql: "datetime2", js: "date"},
			{sql: "datetime", js: "date"},
			{sql: "datetimeoffset", js: "date"},
			{sql: "smalldatetime", js: "date"},
			{sql: "time", js: "date"},

			{sql: "char", js: "string"},
			{sql: "text", js: "string"},
			{sql: "varchar", js: "string"},
			{sql: "nchar", js: "string"},
			{sql: "ntext", js: "string"},
			{sql: "nvarchar", js: "string"},
			{sql: "binary", js: "string"},
			{sql: "image", js: "string"},
			{sql: "varbinary", js: "string"},

			{sql: "cursor", js: "string"},
			{sql: "hierarchyid", js: "string"},
			{sql: "sql_variant", js: "string"},
			{sql: "geography", js: "string"},
			{sql: "table", js: "string"},
			{sql: "rowversion", js: "string"},
			{sql: "uniqueidentifier", js: "string"},
			{sql: "xml", js: "string"},
		],

		chartOptions: {
			chart: {
				height: 300,
				type: 'line',
				zoom: {
					enabled: false
				},
				background: "#ffffff"
			},
			dataLabels: {
				enabled: false
			},
			stroke: {
				curve: 'straight'
			},
			title: {
				text: "",
				align: 'left',
				style: {
					fontFamily: "Roboto"
				}
			},
			grid: {
				row: {
					colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
					opacity: 0.5
				},
			},
		},				
	}),
			
	mounted () {
		const _this = this
				
        _this.logIn(_this.init)
    },
    methods: {
		init: function(){
			const _this = this
			
			_this.getStoredProcedureList()
		},
		logIn: function(_callback){
			const _this = this
			
			_this.username = localStorage.colEmail
			_this.auth_token = localStorage.colAuthToken

			//Keep trying to get the email address from localStorage (loop until it appears there)
			if(!(_this.username) || _this.username == ''){						
				setTimeout(function(){          
					_this.logIn(_this.init)							
				}, 100)
			}else{
			//Once the email address is found in localStorage, execute the callback function
			//Get user's Gmail address from localStorage (set by COLSecurity.js)       
				_callback()
			}
		},
    postData: function(url = '', data = {}){
				return fetch(url, {
						method: "POST",                                
						body: JSON.stringify(data)
				})
				.then(response => response.json())
		},
		getStoredProcedureList: function(){
			const _this = this
				
			_this.postData('https://query.cityoflewisville.com/v2', {
				webservice: "MetricsGetStoredProceduresList",
				auth_token: _this.auth_token,
			})
			.then(data => {	
				_this.storedprocedures = data[0].map(m => {
					return m.name
				})
            
			})
		},				
		getStoredProcedureSchema: function(_spname){
			const _this = this
					
			_this.postData('https://query.cityoflewisville.com/v2', {
				webservice: "MetricsGetStoredProcedureSchema",
				spname: _spname,
				auth_token: _this.auth_token,
			})
			.then(data => {	
					
				//Dataset
          _this.storedproceduredataset = data.dataset
            
				//Schema
					_this.storedprocedureschema = data.schema.map(s => {
							return {
									fieldname: s.FieldName,
									sqltype: s.SystemTypeName,
									jstype: _this._getJsDataType(s.SystemTypeName)
							}
					})
						
				//Headers/Columns
				Object.keys(_this.storedproceduredataset[0]).forEach(function(_obj){
					_this.storedprocedureheaders.push({
						text: (_obj.indexOf("_") == -1 ? _obj : _obj.split("_").join(" ")),
						value: _obj
					})
      	})
            						
				//storedprocedureschemaselected
				_this.storedprocedureschemaselected = _this.storedprocedureheaders.map(m => {
					return m.text
        })            
			})
		},
		_format: function(_format,_value){
			return format(_format,_value)
    },
		_getJsDataType: function(_sqltype){
				const _this = this
				let _result = ''

				const _filter = _this.datatypes.filter(d => {
					return d.sql == _sqltype.split('(')[0]
				})

				if ( _filter.length == 0 ){
					_result = '??'            
				}else{
					_result = _filter[0].js
			}

			return _result
		}
	},
	computed: {
		schemaFieldList: function(){				
			return this.storedprocedureschema.map(m => {
				return m.fieldname
			})
		},
		preview_value: function(){
			const _this = this
			
			if (_this.valuefield.length > 0){
				let _result = 0
				const _values = _this.storedproceduredataset.map(m => {return m[_this.valuefield]})
				const _count = _values.length
				const _sum = _values.reduce((a,b) => parseFloat(a) + parseFloat(b), 0)
				const _avg = (_sum / _count) || 0
				const _latest = _values[_values.length]
				
				if (_this.aggregatetype.toUpperCase() == 'AVERAGE') { _result = _avg }
				if (_this.aggregatetype.toUpperCase() == 'SUM') { _result = _sum }
				if (_this.aggregatetype.toUpperCase() == 'COUNT') { _result = _count }
				if (_this.aggregatetype.toUpperCase() == 'LATEST RECORD') { _result = _latest }
				
				return _result
				
			}else{
				return 0
			}
		},
		goaloperatortypes_array: function(){
			return this.goaloperatortypes.map(m => {
				return m.name + ' (' + m.symbol + ')'
			})
		},
		goaloperatortype: function(){					
			const _this = this
			if (_this.goaloperatortypedisplay.length > 0 ){
				return _this.goaloperatortypedisplay.split(' (')[1].replace(')','')
			}else{
				return ''
			}
		},
		goaloperatortypeabbr: function(){					
			const _this = this
			if (_this.goaloperatortypedisplay.length > 0 ){
				return _this.goaloperatortypes.filter(t => {
					return t.name == _this.goaloperatortypedisplay.split(' (')[0]
				})[0].abbr
			}else{
				return ''
			}
		},
		preview_goalcolor: function(){
			const _this = this
			const _optype = _this.goaloperatortypeabbr
			const _value = _this.preview_value

			const _green1 = _this.color_ranges.green.start
			const _green2 = _this.color_ranges.green.end

			const _yellow1 = _this.color_ranges.yellow.start
			const _yellow2 = _this.color_ranges.yellow.end

			const _red1 = _this.color_ranges.red.start
			const _red2 = _this.color_ranges.red.end

			let _color = 'grey'
			
			//Less Than
			if (_optype == 'lt'){
				if (_value >= _green1 && _value <= _green2 ) { _color = 'green' }
				if (_value >= _yellow1 && _value <= _yellow2 ) { _color = 'yellow' }
				if (_value >= _red1 ) { _color = 'red' }
			}

			//Greater Than
			if (_optype == 'gt'){
				if (_value >= _red1 && _value <= _red2 ) { _color = 'red' }
				if (_value >= _yellow1 && _value <= _yellow2 ) { _color = 'yellow' }
				if (_value >= _green1 ) { _color = 'green' }
			}
			
			return _color
		},
		color_ranges: function(){
			const _this = this
			const _goal = parseFloat(_this.goalvalue)
			const _optype = _this.goaloperatortypeabbr
			const _tolerance = parseFloat(_this.goalwiggleroom)
			const _f = _this.valuefieldformat
			
			let _c = {
				green: 	{start: 0, end: 0, display: '--'},
				yellow: {start: 0, end: 0, display: '--'},
				red:	{start: 0, end: 0, display: '--'},
			}
			
			//Less Than
			if (_optype == 'lt'){ 
				_c.green.start 		= 0
				_c.green.end 		= _goal
				_c.green.display	= format(_f,_c.green.start) + ' to ' + format(_f,_c.green.end)
				
				_c.yellow.start		= _goal + 0.01
				_c.yellow.end		= _goal + (_goal * _tolerance)
				_c.yellow.display	= format(_f,_c.yellow.start) + ' to ' + format(_f,_c.yellow.end)
				
				_c.red.start		= _c.yellow.end + 0.01
				_c.red.end			= -1
				_c.red.display		= format(_f,_c.red.start) + '+'
			}
			
			//Greater Than
			if (_optype == 'gt'){
				_c.green.start 		= _goal
				_c.green.end		= -1
				_c.green.display	= format(_f,_goal) + '+'
				
				_c.yellow.start		= _goal - (_goal * _tolerance) 
				_c.yellow.end		= _goal - 0.01
				_c.yellow.display	= format(_f,_c.yellow.start) + ' to ' + format(_f,_c.yellow.end)
				
				_c.red.start		= 0
				_c.red.end			= _c.yellow.start - 0.01
				_c.red.display		= format(_f,_c.red.start) + ' to ' + format(_f,_c.red.end)
			}
			
			return _c
		},
		series: function(){
			const _this = this
			const _valuefield = _this.valuefield
			const _datefield = _this.datefield
			const _dataset = _this.storedproceduredataset
		
			_dataset.forEach(d=>{
				d["__dateformatted__"] = moment(d[_datefield]).format("MM/DD/yy")
			})

			//data: [{ x: '05/06/2014', y: 54 }, { x: '05/08/2014', y: 17 } , ... , { x: '05/28/2014', y: 26 }]
      const _data = _dataset          
      .map(s => {
				return {
        	x: s["__dateformatted__"], 
          y: s[_valuefield]
        }
      })
      .sort(function(a,b){
       	return new Date(a.x) - new Date(b.x)
      })
          
			return  [{
				name: "Desktops",
				data: _data
			}]
		},
	},
	watch: {		
		title: function(_new){
			//Chart title
			this.chartOptions = {title:{text:_new}}
		},		
	}
};
</script>

<style scoped>
  .v-toolbar{flex-grow: 0;}
  .v-data-table td {font-size: 11px !important}
</style>