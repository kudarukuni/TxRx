<template>
  <div id="pageDashboard" style="min-width: 900px; overflow: auto">
    <client-only
      >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      <v-card class="elevation-1 pa-3">
        <loading :active.sync="isLoading" :can-cancel="false" :width="110" :height="110" backgroundColor="#ffffff" :opacity="0.5" :zIndex="999" :color="API.getLoaderColor"
          :loader="API.getLoader" :is-full-page="fullPage">
        </loading>
        <v-card-text>
          <center>
            <h2 style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif">
              <font color="#3f51b5">
                <b>APPLICATION FOR CONNECTION OF ELECTRICITY SUPPLY</b>
              </font>
            </h2>
            <h6 style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><b>E22</b></h6>
          </center>&nbsp;
          <v-alert type="error" :value="errors.length > 0" :dismissible="true">
            <b>Please correct the following error(s):</b>
            <ul>
              <li v-for="error in errors" :key="error">{{ error }}</li>
            </ul>
          </v-alert>
          <v-alert
            type="success"
            :value="success.length > 0"
            :dismissible="true"
          >
            <p v-for="succ in success" :key="succ">{{ succ }}</p>
          </v-alert>
          <v-form ref="form">
            <v-stepper v-model="e1">
              <v-stepper-header>
                <v-stepper-step :complete="e1 > 1" editable step="1"></v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step :complete="e1 > 2" editable step="2"></v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step :complete="e1 > 3" editable step="3"></v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step :complete="e1 > 4" editable step="4"></v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step :complete="e1 > 5" editable step="5"></v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step :complete="e1 > 6" editable step="6"></v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step :complete="e1 > 7" editable step="7"></v-stepper-step>
                <v-divider></v-divider>
                <v-stepper-step step="8" editable></v-stepper-step>
              </v-stepper-header>

              <v-stepper-items>
                <v-stepper-content step="1">
                  <v-card class="mb-5" height="570px">
                    <v-form ref="form1">
                      <center>
                        <p style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#fc5457"><marquee>Please Choose Customer Type </marquee></font></p>
                      </center>
                      <center>
                        <div class="row justify-content-center">
                          <span class="col-3"></span>
                          <span class="col-3 mb-2 mt-4 modern-checkbox">
                            <input v-model="custype" @click="nextStep(1)" class="ms-1" type="radio" id="indi" value="indi" name="transfer-option"/>
                            <label for="indi"><strong> <font color="#434343">Individual</font></strong><font color="#e96844">*</font></label>
                          </span>
                          <span class="col-2 ms-1 mb-2 mt-4 modern-checkbox">
                            <input v-model="custype" @click="nextStep(1)" class="ms-1" type="radio" id="org" value="org" name="transfer-option"/>
                            <label for="org"><strong> <font color="#434343">Organisation</font></strong><font color="#e96844">*</font></label>
                          </span>
                          <span class="col-3"></span>
                        </div>
                      </center>
                    </v-form>                    
                  </v-card>
                  <center><v-btn style="margin-top: 20px" outlined big color="red" :href="'/indyconn'">Cancel</v-btn></center>
                </v-stepper-content>

                <v-stepper-content step="2">
                  <v-card class="mb-5" height="550px">
                    <v-form ref="form2">
                      <v-flex xs12 d-flex>
                        <v-autocomplete sx6 dense v-if="custype === 'indi'" :items="gender" v-model="model.gender" label="Title" required item-text="gender" item-value="abbr" class="input-group--focused" style="margin: 1em; width: 10px;" :rules="[(v) => !!v || 'Title is required']"></v-autocomplete>
                        <v-text-field sx6 dense v-if="custype === 'indi'" name="input-1" label="Surname" color="primary" v-model="model.surname" required id="testing" style="margin: 1em; width: 10px;" :rules="[(v) => !!v || 'Surname is required']"></v-text-field>
                        <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Name Of Company" color="primary" v-model="model.company_name" required id="testing" style="margin: 1em; width: 10px;" :rules="[(v) => !!v || 'Name Of Company is required']"></v-text-field>
                        <v-text-field xs6 dense v-if="custype === 'org'" name="input-1" label="Company Postal Address" required color="primary" v-model="model.cpostal_address_a" style="margin: 1em; width: 10px;" :rules="[(v) => !!v || 'Company Postal Address is required']"></v-text-field>                        
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <v-text-field sx6 dense v-if="custype === 'indi'" name="input_1" label="Firstname" color="primary" v-model="model.customer_name" required id="testing" style="margin: 1em; width: 10px;" :rules="[(v) => !!v || 'Firstname is required']"></v-text-field>
                        <v-text-field sx6 dense v-if="custype === 'indi'" name="input_1" placeholder="e.g 12-345678-A-90" label="National ID" color="primary" v-model="model.document_id" required id="testing" style="margin: 1em; width: 10px;" :rules="[(v) => !!v || 'National ID is required', (v) => /^[0-9]{2}-[0-9]{6,7}-[a-zA-Z]-[0-9]{2}$/.test(v) || 'National ID e.g 12-345678-A-90']" maxlength="15"></v-text-field>
                        <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Certification Of Incorporation No." color="primary" v-model="model.cert_of_inc_number" required id="testing" style="margin: 1em; width: 10px;" :rules="[(v) => !!v || 'Certification Of Incorporation No. is required']"></v-text-field>
                        <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" type="email" label="Company Email Address" v-model="model.email" :rules="[v => /.+@.+\..+/.test(v) || 'E-mail must be valid']" color="primary" id="testing" style="margin: 1em; width: 10px;"></v-text-field>                        
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <v-text-field xs12 dense v-if="custype === 'indi'" name="input-1" label="Postal Address" required color="primary" v-model="model.postal_address_a" style="margin: 1em" :rules="[(v) => !!v || 'Postal Address is required']"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <VuePhoneNumberInput sx6 v-if="custype === 'indi'" dense v-model="model.owner_contact1" label="Cellphone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided',]"/>
                        <v-text-field sx6 v-if="custype === 'indi'" dense v-model="model.owner_contact2" label="Landline Number" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUp" maxlength="9" :rules="[(v) => !!v || 'Your Phone Number is required', (v) => v !== model.owner_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact1 || 'Phone Number Must be Different From the Already Provided', , (v) => v !== model.employed_contact2 || 'Cellphone Number Must Differ From Landline Number']"></v-text-field>

                        <VuePhoneNumberInput sx6 v-if="custype === 'org'" dense v-model="model.company_phone1" label="Cellphone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7',
 (v) => v !== model.company_phone2 || 'Phone Number Must be Different From the Already Provided']"/>
                        <v-text-field sx6 v-if="custype === 'org'" dense v-model="model.company_phone2" label="Landline Number" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUp" maxlength="9" :rules="[(v) => !!v || 'Your Phone Number is required',
 (v) => v !== model.company_phone1 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                      </v-flex>
                      <v-flex xs6 d-flex>
                        <v-text-field readonly sx6 v-if="custype === 'indi'" dense v-model="model.owner_contact1" label="" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact1 || 'Phone Number Must be Different From the Already Provided', , (v) => v !== model.employed_contact2 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                        <v-text-field readonly sx6 v-if="custype === 'org'" dense v-model="model.company_phone1" label="" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.company_phone2 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                      </v-flex>
                      <v-flex v-if="custype === 'indi'" xs12 d-flex>
                        <div class="container">
                          <form enctype="multipart/form-data" novalidate v-if="isInitial1 || isSaving1">
                            <h5 align="center" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;">
                              <b>Upload Your ID</b>
                            </h5>
                            <div class="dropbox">                              
                              <input type="file" single :name="uploadFieldName1" :disabled="isSaving1" @change="filesChange1($event.target.name, $event.target.files);
                                fileCount = $event.target.files.length;" accept="application/pdf, image/*" class="input-file" required/>
                              <p v-if="isInitial1">Drag & Drop Your File Here<br/>Or<br/>Click To Browse ....</p>
                              <p v-if="isSaving1">
                                Uploading Files, Please Wait ....
                              </p>
                            </div>
                            <p>                              
                              <b>
                                <li>
                                  <i>Zimbabwean National ID <font color="red">*</font></i>
                                </li>
                              </b>
                            </p>
                          </form>
                          <div v-if="isSuccess1">
                            <font color="green" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><center>
                              Premise Owner ID Uploaded Successfully
                            </center></font>
                              <div v-for="(item1, index) in uploadedFiles1" style="display: flex; align-items: center;">
                                <v-text-field style="width: 35px; color: red; margin-right: 435px;" class="text-field-transparent" flat name="owner_id_proof" type="text" autocomplete="off"
                                  v-model="model.owner_id_proof = item1.id" readonly :rules="[(v) => !!v || 'Owner ID Proof is required',]"></v-text-field>
                                <v-text-field sx6 style="width: 200px; color: red;" class="text-field-transparent" flat name="originalName" type="text" autocomplete="off" 
                                  :placeholder="item1.originalName" readonly></v-text-field><br/>                                
                              </div>
                              <br/><a href="javascript:void(0)" @click="resetme1()">Made A Mistake ??</a>
                          </div>
                          <div v-if="isFailed1">
                            <h3 align="right" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><font color="red">Failed To Upload Document !!</font> </h3>
                            <p><a href="javascript:void(0)" @click="reset1()" >Please Try Again ....</a></p>
                            <pre><p><strong><b>Please upload PDF documents only and try again.<br/> If the problem persists please call #704 ZETDC Call Center</b></strong></p></pre>
                          </div>
                        </div>                         
                      </v-flex>
                      <v-flex v-if="custype === 'org'" xs12 d-flex>
                        <div class="container">
                          <form enctype="multipart/form-data" novalidate v-if="isInitial1 || isSaving1">
                            <h5 align="center" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;">
                              <b>Upload Your Company Documents</b>
                            </h5>
                            <div class="dropbox">
                              <input type="file" single :name="uploadFieldName1" :disabled="isSaving1" @change="filesChange1($event.target.name, $event.target.files);
                                fileCount = $event.target.files.length;" accept="application/pdf, image/png, image/jpeg, image/jpg" class="input-file" required/>
                              <p v-if="isInitial1">Drag & Drop Your File Here<br/>Or<br/>Click To Browse ....</p>
                              <p v-if="isSaving1">
                                Uploading Files, Please Wait ....
                              </p>
                            </div>
                            <p>                              
                              <b>
                                <li>
                                  <i>CR5 Documents <font color="red">*</font></i>
                                </li>
                              </b>
                            </p>
                          </form>
                          <div v-if="isSuccess1">
                            <font color="green" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><center>
                              Company Documents Uploaded Successfully
                            </center></font>
                              <div v-for="(item1, index) in uploadedFiles1" style="display: flex; align-items: center;">
                                <v-text-field style="width: 35px; color: red; margin-right: 435px;" class="text-field-transparent" flat name="owner_id_proof" type="text" autocomplete="off"
                                  v-model="model.owner_id_proof = item1.id" readonly :rules="[(v) => !!v || 'Owner ID Proof is required',]"></v-text-field>
                                <v-text-field sx6 style="width: 200px; color: red;" class="text-field-transparent" flat name="originalName" type="text" autocomplete="off" 
                                  :placeholder="item1.originalName" readonly></v-text-field><br/>                                
                              </div>
                              <br/><a href="javascript:void(0)" @click="resetme1()">Made A Mistake ??</a>
                          </div>
                          <div v-if="isFailed1">
                            <h3 align="right" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><font color="red">Failed To Upload Document !!</font> </h3>
                            <p><a href="javascript:void(0)" @click="reset1()" >Please Try Again ....</a></p>
                            <pre><p><strong><b>Please upload PDF documents only and try again.<br/> If the problem persists please call #704 ZETDC Call Center</b></strong></p></pre>
                          </div>
                        </div>                         
                      </v-flex>
                    </v-form>
                  </v-card>
                  <v-btn color="primary" @click="nextStep(2)">Continue</v-btn>
                  <v-btn color="green" @click="prevStep(2)">Back</v-btn><br>
                  <center><v-btn style="margin-top: 20px" :href="'/indyconn'" outlined big color="red">Cancel</v-btn></center>
                </v-stepper-content>

                <v-stepper-content step="3">
                  <v-card class="mb-5" height="570px">
                    <v-form ref="form3">
                      <v-flex xs12 d-flex style="height: 60px;">
                        <v-text-field sx6 v-if="custype === 'indi'" name="input-1" label="Next of Kin Firstname" required color="primary" v-model="model.nok_firstname" style="margin: 1em"
                          :rules="[v => /^([a-zA-Z]+((['][a-zA-Z ])?[a-zA-Z]*)){2,30}$/.test(v) || 'Invalid Firstname']"
                        ></v-text-field>
                        <v-text-field sx6 v-if="custype === 'indi'" name="input-1" label="Next of Kin Surname" required color="primary" v-model="model.nok_surname" style="margin: 1em"
                          :rules="[v => /^([a-zA-Z]+((['][a-zA-Z ])?[a-zA-Z]*)){2,30}$/.test(v) || 'Invalid Surname']"
                        ></v-text-field>
                        <v-text-field sx6 v-if="custype === 'org'" name="input-1" label="Shareholders Surname" color="primary" v-model="model.rep_surname" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders Surname is required']"></v-text-field>
                        <v-text-field sx6 v-if="custype === 'org'" name="input-1" label="Shareholders Firstname" color="primary" v-model="model.rep_firstname" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders Firstname is required']"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <v-text-field sx6 v-if="custype === 'indi'" name="input_1" placeholder="e.g 12-345678-A-90" label="Next of Kin National ID" color="primary" v-model="model.nok_national_id" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Next of Kin National ID is required', (v) => /^[0-9]{2}-[0-9]{6,7}-[a-zA-Z]-[0-9]{2}$/.test(v) || 'Match Document ID with 12-345678-A-90']" maxlength="15"></v-text-field>
                        <v-text-field sx6 v-if="custype === 'indi'" name="input-1" label="Next of Kin Physical Address" required color="primary" v-model="model.nok_address" style="margin: 1em" :rules="[(v) => !!v || 'Next Of Kin Address is required',]"></v-text-field>
                        <v-text-field sx6 v-if="custype === 'org'" name="input-1" label="Shareholders National ID" color="primary" v-model="model.rep_national_id" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders National ID is required']"></v-text-field>
                        <v-text-field sx6 v-if="custype === 'org'" name="input-1" label="Shareholders Postal Address" color="primary" v-model="model.rep_postal_address" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders Postal Address is required']"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <VuePhoneNumberInput sx6 v-if="custype === 'indi'" dense v-model="model.nok_contact1" label="Cellphone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 110px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.nok_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact2 || 'Phone Number Must be Different From the Already Provided']"/>
                        <v-text-field sx6 v-if="custype === 'indi'" dense v-model="model.nok_contact2" label="Landline" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUp" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.nok_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact2 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                        <VuePhoneNumberInput sx6 v-if="custype === 'org'" dense v-model="model.rep_contact1" label="Cellphone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 110px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.rep_contact2 || 'Phone Number Must be Different From the Already Provided']"/>
                        <v-text-field sx6 v-if="custype === 'org'" dense v-model="model.rep_contact2" label="Landline" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUp" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.rep_contact1 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                      </v-flex>
                      <v-flex xs6 d-flex>
                        <v-text-field readonly sx6 v-if="custype === 'indi'" dense v-model="model.nok_contact1" label="" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.nok_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.employed_contact2 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                        <v-text-field readonly sx6 v-if="custype === 'org'" dense v-model="model.rep_contact1" label="" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 10px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.rep_contact2 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                      </v-flex>
                      <v-flex v-if="custype === 'indi'" xs12 d-flex>
                        <div class="container">                          
                          <form enctype="multipart/form-data" novalidate v-if="isInitial2 || isSaving2">
                            <h5 align="center" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;">
                              <br/><b>Upload Next of Kin ID</b>
                            </h5>
                            <div class="dropbox">
                              <input type="file" multiple :name="uploadFieldName2" :disabled="isSaving2" @change="filesChange2($event.target.name, $event.target.files);
                                fileCount = $event.target.files.length;" accept="application/pdf, image/png, image/jpeg, image/jpg" class="input-file"/>
                              <p v-if="isInitial2">Drag & Drop Your File Here<br/>Or<br/>Click To Browse ....</p>
                              <p v-if="isSaving2">
                                Uploading Files, Please Wait ....
                              </p>
                            </div>
                            <p>                              
                              <b>
                                <li>
                                  <i>Zimbabwean National ID <font color="red">*</font></i>
                                </li>
                              </b>
                            </p>
                          </form>

                          <div v-if="isSuccess2">
                            <font color="green" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><center>
                              Proof Of Residence Uploaded Successfully
                            </center></font>
                              <div v-for="(item2, index) in uploadedFiles2" style="display: flex; align-items: center;">
                                <v-text-field style="width: 35px; color: red; margin-right: 435px;" class="text-field-transparent" flat name="nok_id_proof" type="text" autocomplete="off"
                                  v-model="model.nok_id_proof = item2.id" readonly :rules="[(v) => !!v || 'Next Of Kin ID is required',]"></v-text-field>
                                <v-text-field sx6 style="width: 200px; color: red;" class="text-field-transparent" flat name="originalName" type="text" autocomplete="off" 
                                  :placeholder="item2.originalName" readonly></v-text-field><br/>                                
                              </div>
                              <br/><a href="javascript:void(0)" @click="resetme2()">Made A Mistake ??</a>
                          </div>
                          <div v-if="isFailed2">
                            <h3 align="right" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><font color="red">Failed To Upload Document !!</font> </h3>
                            <p><a href="javascript:void(0)" @click="reset2()" >Please Try Again ....</a></p>
                            <pre><p><strong><b>Please upload PDF documents only and try again.<br/> If the problem persists please call #704 ZETDC Call Center</b></strong></p></pre>
                          </div>
                        </div>
                      </v-flex>
                      <v-flex v-if="custype === 'org'" xs12 d-flex>
                        <div class="container">                          
                          <form enctype="multipart/form-data" novalidate v-if="isInitial2 || isSaving2">
                            <h5 align="center" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;">
                              <br/><b>Upload Shareholders National ID</b>
                            </h5>
                            <div class="dropbox">
                              <input type="file" multiple :name="uploadFieldName2" :disabled="isSaving2" @change="filesChange2($event.target.name, $event.target.files);
                                fileCount = $event.target.files.length;" accept="application/pdf, image/png, image/jpeg, image/jpg" class="input-file"/>
                              <p v-if="isInitial2">Drag & Drop Your File Here<br/>Or<br/>Click To Browse ....</p>
                              <p v-if="isSaving2">
                                Uploading Files, Please Wait ....
                              </p>
                            </div>
                            <p>                              
                              <b>
                                <li>
                                  <i>Zimbabwean National ID <font color="red">*</font></i>
                                </li>
                              </b>
                            </p>
                          </form>
                          <div v-if="isSuccess2">
                            <font color="green" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><center>
                              Shareholders National ID Uploaded Successfully
                            </center></font>
                              <div v-for="(item2, index) in uploadedFiles2" style="display: flex; align-items: center;">
                                <v-text-field style="width: 35px; color: red; margin-right: 435px;" class="text-field-transparent" flat name="nok_id_proof" type="text" autocomplete="off"
                                  v-model="model.nok_id_proof = item2.id" readonly :rules="[(v) => !!v || 'Next Of Kin ID is required',]"></v-text-field>
                                <v-text-field sx6 style="width: 200px; color: red;" class="text-field-transparent" flat name="originalName" type="text" autocomplete="off" 
                                  :placeholder="item2.originalName" readonly></v-text-field><br/>                                
                              </div>
                              <br/><a href="javascript:void(0)" @click="resetme2()">Made A Mistake ??</a>
                          </div>
                          <div v-if="isFailed2">
                            <h3 align="right" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><font color="red">Failed To Upload Document !!</font> </h3>
                            <p><a href="javascript:void(0)" @click="reset2()" >Please Try Again ....</a></p>
                            <pre><p><strong><b>Please upload PDF documents only and try again.<br/> If the problem persists please call #704 ZETDC Call Center</b></strong></p></pre>
                          </div>
                        </div>
                      </v-flex>
                    </v-form>
                  </v-card>
                  <v-btn color="primary" v-if="custype === 'indi'" @click="nextStep(3)">Continue</v-btn>
                  <v-btn color="primary" v-if="custype === 'org'" @click="nextStep(4)">Continue</v-btn>
                  <v-btn color="green" @click="prevStep(3)">Back</v-btn><br/>
                  <center><v-btn style="margin-top: 20px" outlined big color="red" :href="'/indyconn'">Cancel</v-btn></center>
                </v-stepper-content>

                <v-stepper-content step="4">
                  <v-card class="mb-5" height="570px">
                    <v-form ref="form4">
                      <center>
                        <p style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#fc5457">Please Tell Us If You Are Employed</font></p>
                      </center>
                      <center><div class="row justify-content-center">
                        <span class="col-3"></span>
                        <span class="col-3 mb-2 mt-4 modern-checkbox">
                          <input
                            v-model="employed"
                            class="ms-1"
                            type="radio"
                            id="work"
                            value="work"
                            name="transfer-option"
                          />
                          <label for="yes"><strong> <font color="#434343">Yes</font></strong><font color="#e96844">*</font></label>
                        </span>
                        <span class="col-2 ms-1 mb-2 mt-4 modern-checkbox">
                          <input
                            v-model="employed"
                            class="ms-1"
                            type="radio"
                            id="home"
                            value="home"
                            name="transfer-option"
                          />
                          <label for="no"><strong> <font color="#434343">No</font></strong><font color="#e96844">*</font></label>
                        </span>
                        <span class="col-3"></span>
                      </div></center>
                      <v-flex xs12 d-flex>
                        <v-text-field sx6 name="input-1" label="Name of Employer" color="primary" v-model="model.name_of_employer" v-if="employed === 'work'" id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Name of Employer is required',]"></v-text-field>
                        <v-text-field sx6 name="input-1" label="Address Of Employer" color="primary" v-model="model.employer_address" v-if="employed === 'work'" id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Employers Address is required',]"></v-text-field>                        
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <VuePhoneNumberInput sx6 v-model="model.employed_contact1" label="Employer Cellphone" required default-country-code="ZW" valid-color="green" v-if="employed === 'work'" style="margin: 1em; width: 110px" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.employed_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact1 || 'Phone Number Must be Different From the Already Provided']" @update="onUpdate2" maxlength="9"/>
                        <v-text-field readonly sx6 v-if="employed === 'work'" dense v-model="model.employed_contact2" label="Employer Landline" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUp2" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.employed_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact1 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <v-text-field readonly sx6 v-if="employed === 'work'" dense v-model="model.employed_contact1" label="" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUpdate2" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.employed_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.nok_contact1 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact2 || 'Phone Number Must be Different From the Already Provided', (v) => v !== model.owner_contact1 || 'Phone Number Must be Different From the Already Provided']"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>                        
                        <v-text-field sx6 name="input-1" label="Premise Owner / Landlord Name" color="primary" v-model="model.premise_owner_name" id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Premise Owner / Landlord Name is required',]"></v-text-field>
                        <v-text-field sx6 name="input-1" label="Premise Owner / Landlord Surname" color="primary" v-model="model.premise_owner_surname" id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Premise Owner / Landlord Surname is required',]"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <v-text-field sx6 name="input-1" label="Premise Owner / Landlord Address" color="primary" v-model="model.premise_owner_address" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Premise Owner / Landlord Address is required']"></v-text-field>
                      </v-flex>
                    </v-form>
                  </v-card>
                  <v-btn color="primary" @click="nextStep(4)" :loading="loading">Continue</v-btn>
                  <v-btn color="green" @click="prevStep(4)">Back</v-btn><br/>
                  <center><v-btn style="margin-top: 20px" outlined big color="red" :href="'/indyconn'">Cancel</v-btn></center>
                </v-stepper-content>

                <v-stepper-content step="5">
                  <v-card class="mb-5" height="570px">
                    <v-form ref="form5">
                      <center><p style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#fc5457">Please choose your requested service</font></p></center>
                      <center><div class="row justify-content-center">
                        <span class="col-3"></span>
                        <span class="col-3 mb-2 mt-4 modern-checkbox">
                          <input v-model="rservice" class="ms-1" type="radio" id="connect" value="connect" name="transfer-option" @click="nextStep(5)"/>
                          <label for="yes"><strong><font color="#434343">Connect</font></strong><font color="#e96844">*</font></label>
                        </span>
                        <span class="col-2 ms-1 mb-2 mt-4 modern-checkbox">
                          <input v-model="rservice" class="ms-1" type="radio" id="disconnect" value="disconnect" name="transfer-option" @click="nextStep(5)"/>
                          <label for="no"><strong><font color="#434343">Disconnect</font></strong><font color="#e96844">*</font></label>
                        </span>
                        <span class="col-3"></span>
                      </div></center>                      
                    </v-form>
                  </v-card>
                  <v-btn color="primary" @click="nextStep(5)" :loading="loading">Continue</v-btn>
                  <v-btn color="green" @click="prevStep(5)">Back</v-btn><br/>
                  <center><v-btn style="margin-top: 20px" outlined big color="red" :href="'/indyconn'">Cancel</v-btn></center>
                </v-stepper-content>

                <v-stepper-content step="6">
                  <v-card class="mb-5" height="570px">
                    <v-form ref="form6">
                      <v-flex xs12 d-flex>
                        <v-select xs6 dense v-model="model.town" :items=sortFunc() item-text="nom_MUNIC" item-value="cod_MUNIC" label="Town" required @change="surbubName($event)" :rules="[v => !!v || 'Town  is required']" class="input-group--focused" style="margin: 1em;"></v-select>
                        <v-select xs6 dense :items=sortSub() v-model="model.surbub" label="Suburb/Township" required @change="streetName($event)" :rules="[v => !!v || 'Surbub  is required']" item-text="nom_LOCAL" item-value="cod_LOCAL" class="input-group--focused" style="margin: 1em;"></v-select>
                      </v-flex>
                      <v-flex xs12 d-flex>                        
                        <v-select xs6 dense v-if="rservice === 'connect'" :items=sortStr() v-model="model.street" label="Farm/Street Name" item-text="nom_CALLE" item-value="cod_CALLE" required :rules="[v => !!v || 'Street  is required']" class="input-group--focused" style="margin: 1em;"></v-select>
                        <v-text-field sx6 dense v-if="rservice === 'disconnect'" name="input-1" label="Physical Address" color="primary" v-model="model.physical_address" id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Physical Address is required',]"></v-text-field>
                        <v-autocomplete dense sx6 :items="nonstarnd" v-model="model.nonstarnd" label="Application Type" :rules="[v => !!v || 'Application Type is required']" item-text="nonstarnd" item-value="abbr" class="input-group--focused" style="margin: 1em;" required></v-autocomplete>
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <v-text-field xs6 dense name="standnumber" type="number" label="Stand/Plot Number" color="primary" :maxlength="max" v-model="model.standnumber" required style="margin: 1em;" :rules="[v => !!v || 'Stand Number  is required']"></v-text-field>
                        <v-text-field xs6 dense name="door_number" type="number" label="Street Number" color="primary"
                          :maxlength="max" v-model="model.door_number" style="margin: 1em;" required @input="validateDoorNumber"></v-text-field>
                        </v-flex>                      
                      <v-flex xs6 d-flex>
                        <v-text-field xs6 dense name="duplicator" label="Extension" color="primary" :maxlength="max" v-model="model.duplicator" style="margin: 1em;" required @input="validateDoorNumber"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>
                        <div class="container" style="margin-top: 1">
                          <form enctype="multipart/form-data" novalidate v-if="isInitial || isSaving">
                            <h5 align="center" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;">
                              <b>Upload Proof Of Residence</b>
                            </h5>
                            <div class="dropbox">
                              <input type="file" multiple :name="uploadFieldName" :disabled="isSaving" @change="filesChange($event.target.name, $event.target.files);
                                fileCount = $event.target.files.length;" accept="application/pdf, image/png, image/jpeg, image/jpg" class="input-file"/>
                              <p v-if="isInitial">Drag & Drop Your File Here<br/>Or<br/>Click To Browse ....</p>
                              <p v-if="isSaving">
                                Uploading Files, Please Wait ....
                              </p>
                            </div>
                            <p>                              
                              <b>
                                <li>
                                  <i>Proof Of Residence <font color="red">*</font></i>&nbsp;&nbsp;&nbsp;&nbsp;<i>Affidavit <font color="red">*</font></i>&nbsp;&nbsp;&nbsp;&nbsp;<i>Leese Agreement <font color="red"><a href="https://portfolio-kudarukuni.vercel.app" target="_blank"><b>*</b></a></font></i>
                                </li>
                              </b>
                            </p>
                          </form>
                          <div v-if="isSuccess">
                            <font color="green" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><center>
                              Proof Of Residence Uploaded Successfully
                            </center></font>
                              <div v-for="(item, index) in uploadedFiles" style="display: flex; align-items: center;">
                                <v-text-field dense style="width: 35px; color: red; margin-right: 435px;" class="text-field-transparent" flat name="postal_proof" type="text" autocomplete="off"
                                  v-model="model.postal_proof = item.id" readonly :rules="[(v) => !!v || 'Proof Of Residence is required',]"></v-text-field>
                                <v-text-field dense sx6 style="width: 200px; color: red;" class="text-field-transparent" flat name="postal_proof" type="text" autocomplete="off" 
                                  :placeholder="item.originalName" readonly></v-text-field><br/>                                
                              </div>
                              <br/><a href="javascript:void(0)" @click="resetme()">Made A Mistake ??</a>
                          </div>
                          <div v-if="isFailed">
                            <h3 align="right" style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif;"><font color="red">Failed To Upload Document !!</font> </h3>
                            <p><a href="javascript:void(0)" @click="reset()" >Please Try Again ....</a></p>
                            <pre><p><strong><b>Please upload PDF documents only and try again.<br/> If the problem persists please call #704 ZETDC Call Center</b></strong></p></pre>
                          </div>
                        </div>
                      </v-flex>
                    </v-form>
                  </v-card>
                  <v-btn color="primary" @click="nextStep(6)">Continue</v-btn>
                  <v-btn color="green" @click="prevStep(6)">Back</v-btn><br/>
                  <center><v-btn style="margin-top: 20px" outlined big color="red" :href="'/indyconn'">Cancel</v-btn></center>
                </v-stepper-content>

                <v-stepper-content step="7">
                  <v-card class="mb-5" height="570px">
                    <v-form ref="form7">
                      <center>
                        <p style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif">
                          <font color="#fc5457">Should Final Account Be Transfered To New Account?</font>
                        </p>
                      </center>
                      <center>
                        <div class="row justify-content-center">
                          <span class="col-3"></span>
                          <span class="col-3 mb-2 mt-4 modern-checkbox">
                            <input v-model="newaccount" class="ms-1" type="radio" id="allow" value="allow" name="transfer-option"/><label for="yes">
                              <strong> <font color="#434343"> Yes</font></strong>
                              <font color="#e96844">*</font>
                            </label>
                          </span>
                          <span class="col-2 ms-1 mb-2 mt-4 modern-checkbox">
                            <input v-model="newaccount" class="ms-1" @click="nextStep(7)" type="radio" id="disallow" value="disallow" name="transfer-option"/>
                            <label for="no">
                              <strong> 
                                <font color="#434343"> No</font>
                              </strong>
                              <font color="#e96844">*</font>
                            </label>
                          </span>
                          <span class="col-3"></span>
                        </div>
                      </center>
                      <v-flex xs12 d-flex>                        
                        <v-text-field sx6 name="input-1" label="Account Number" v-if="newaccount === 'allow'" color="primary" v-model="model.nis_rad" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Old Account Number is required',]"></v-text-field>
                        <v-text-field sx6 name="input-1" label="Postal Address" color="primary" :rules="[(v) => !!v || 'Postal Address is required',]" v-model="model.postal_address_a" required id="testing" style="margin: 1em;" v-if="newaccount === 'allow' && rservice === 'connect'"></v-text-field>
                        <v-text-field sx6 name="input-1" label="Address To Which Final Account Must Be Sent" color="primary" v-model="model.service_point_address" required id="testing" style="margin: 1em" v-if="newaccount === 'allow' && rservice === 'disconnect'"></v-text-field>
                      </v-flex>
                      <v-flex xs12 d-flex>                        
                        <v-text-field sx6 name="input-1" placeholder="(IF KNOWN)" label="Reciept Number" color="primary" v-model="model.reciepts" required id="testing" style="margin: 1em" v-if="newaccount === 'allow' && rservice === 'connect'"></v-text-field>
                        <v-text-field sx6 name="input-1" label="Security Deposit" color="primary" v-model="model.security_deposit" required id="testing" style="margin: 1em" v-if="newaccount === 'allow' && rservice === 'connect'"></v-text-field>
                        </v-flex>
                      <v-flex xs12 d-flex>
                        <v-autocomplete sx6 v-if="newaccount === 'allow' && rservice === 'connect'" :items="reasonforconn" v-model="model.reasonforconn" label="Reason For Reconnection" required item-text="reasonforconn" item-value="abbr" class="input-group--focused" style="margin: 1em" :rules="[(v) => !!v || 'Reason For Reconnection is required']"></v-autocomplete>
                        <v-autocomplete sx6 v-if="newaccount === 'allow' && rservice === 'disconnect'" :items="reasonfordisconn" v-model="model.reasonfordisconn" label="Reason For Disconnection" required item-text="reasonfordisconn" item-value="abbr" class="input-group--focused" style="margin: 1em" :rules="[(v) => !!v || 'Reason For Disconnection is required']"></v-autocomplete>
                        <div v-if="newaccount === 'allow'" style="display: flex; justify-content: center; align-items: center; width: 50%; margin-bottom: 1rem;">
                          <label for="date-input" style="margin-right: 1rem; font-weight: bold;">Date Required</label>
                          <input type="date" v-model="model.date_required" id="date-input" style="margin-right: 1rem; padding: 1.0rem; border: 1px solid #ccc; border-radius: 4px; width: 61%;">
                        </div>                        
                      </v-flex>
                        
                    </v-form>
                  </v-card>
                  <v-btn color="primary" @click="nextStep(7)">Continue</v-btn>
                  <v-btn color="green" @click="prevStep(7)">Back</v-btn><br/>
                  <center><v-btn style="margin-top: 20px" outlined big color="red" :href="'/indyconn'">Cancel</v-btn></center>
                </v-stepper-content>

                <v-stepper-content step="8">
                  <v-card class="mb-5" height="1800px">
                    <v-form ref="form8">
                      <v-card-text>
                        <div ref="printContent" class="print-content">
                          <v-form class="form">
                            <div class="form-column">
                              <v-text-field sx6 dense v-if="custype === 'indi'" :items="gender" v-model="model.gender" label="Title" required item-text="gender" item-value="abbr" class="input-group--focused" style="margin: 1em" :rules="[(v) => !!v || 'Title is required']"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Name Of Company" color="primary" v-model="model.company_name" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Name Of Company is required']"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'indi'" name="input_1" placeholder="e.g 12-345678-A-90" label="National ID" color="primary" v-model="model.document_id" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'National ID is required', (v) => /^[0-9]{2}[0-9]{6,7}[a-zA-Z]-[0-9]{2}$/.test(v) || 'National ID e.g 12-345678-A-90']" maxlength="15"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Certification Of Incorporation No." color="primary" v-model="model.cert_of_inc_number" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Certification Of Incorporation No. is required']"></v-text-field>
                            </div>
                            <div class="form-column">
                              <v-text-field sx6 dense v-if="custype === 'indi'" name="input-1" label="Surname" color="primary" v-model="model.surname" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Surname is required']"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" type="email" label="Company Email Address" v-model="model.email" :rules="[v => /.+@.+\..+/.test(v) || 'E-mail must be valid']" color="primary" id="testing" style="margin: 1em;"></v-text-field>
                              <v-text-field readonly sx6 v-if="custype === 'indi'" dense v-model="model.owner_contact1" label="Phone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.owner_contact2 || 'Phone numbers must be different']"></v-text-field>
                              <v-text-field readonly sx6 v-if="custype === 'org'" dense v-model="model.company_phone1" label="Phone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUpdate" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.company_phone2 || 'Phone numbers must be different']"></v-text-field>
                            </div>
                            <div class="form-column">
                              <v-text-field sx6 dense v-if="custype === 'indi'" name="input_1" label="Firstname" color="primary" v-model="model.customer_name" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Firstname is required']"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Shareholders Surname" color="primary" v-model="model.rep_surname" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders Surname is required']"></v-text-field>
                            </div>
                          </v-form>
                          <v-form class="form">
                            <div class="form-column">
                              <v-text-field xs12 dense v-if="custype === 'indi'" name="input-1" label="Postal Address" required color="primary" v-model="model.postal_address_a" style="margin: 1em" :rules="[(v) => !!v || 'Postal Address is required']"></v-text-field>
                              <v-text-field xs6 dense v-if="custype === 'org'" name="input-1" label="Company Postal Address" required color="primary" v-model="model.cpostal_address_a" style="margin: 1em" :rules="[(v) => !!v || 'Company Postal Address is required']"></v-text-field>
                            </div>
                          </v-form>
                          <v-form class="form">
                            <div class="form-column">
                              <v-text-field readonly sx6 v-if="custype === 'indi'" dense v-model="model.owner_contact2" label="Landline" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUp" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.owner_contact1 || 'Phone numbers must be different']"></v-text-field>
                              <v-text-field readonly sx6 v-if="custype === 'org'" dense v-model="model.company_phone2" label="Landline" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUp" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.company_phone1 || 'Phone numbers must be different']"></v-text-field>
                              <v-text-field xs6 dense v-if="rservice === 'connect'" :items=sortStr() v-model="model.street" label="Farm/Street Name" item-text="nom_CALLE" item-value="cod_CALLE" required :rules="[v => !!v || 'Street  is required']" class="input-group--focused" style="margin: 1em;"></v-text-field>
                              <v-text-field sx6 dense v-if="rservice === 'disconnect'" name="input-1" label="Physical Address" color="primary" v-model="model.physical_address" id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Physical Address is required',]"></v-text-field>
                              <v-text-field xs6 dense name="duplicator" label="Extension" color="primary" :maxlength="max" v-model="model.duplicator" style="margin: 1em;" required @input="validateDoorNumber"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Shareholders National ID" color="primary" v-model="model.rep_national_id" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders National ID is required']"></v-text-field>
                            </div>
                            <div class="form-column">
                              <v-select xs6 dense v-model="model.town" :items=sortFunc() item-text="nom_MUNIC" item-value="cod_MUNIC" label="Town" required @change="surbubName($event)" :rules="[v => !!v || 'Town  is required']" class="input-group--focused" style="margin: 1em;"></v-select>
                              <v-text-field xs6 dense name="standnumber" type="number" label="Stand/Plot Number" color="primary" :maxlength="max" v-model="model.standnumber" required style="margin: 1em;" :rules="[v => !!v || 'Stand Number  is required']"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'indi'" name="input-1" label="Next of Kin Surname" required color="primary" v-model="model.nok_surname" style="margin: 1em"
                                :rules="[v => /^([a-zA-Z]+((['][a-zA-Z ])?[a-zA-Z]*)){2,30}$/.test(v) || 'Invalid Surname']"
                              ></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Shareholders Surname" color="primary" v-model="model.rep_surname" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders Surname is required']"></v-text-field>
                              <v-text-field readonly sx6 v-if="custype === 'indi'" dense v-model="model.nok_contact1" label="Phone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUpdate1" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.nok_contact2 || 'Phone numbers must be different']"></v-text-field>
                              <v-text-field readonly sx6 v-if="custype === 'indi'" dense v-model="model.nok_contact2" label="Landline" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUp1" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.nok_contact1 || 'Phone numbers must be different']"></v-text-field>
                              <v-text-field readonly sx6 v-if="custype === 'org'" dense v-model="model.rep_contact1" label="Phone Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUpdate1" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.rep_contact2 || 'Phone numbers must be different']"></v-text-field>
                              <v-text-field readonly sx6 v-if="custype === 'org'" dense v-model="model.rep_contact2" label="Landline" default-country-code="ZW" valid-color="green" style="margin: 1em; width: 70px" @update="onUp1" maxlength="9" :rules="[(v) => (v && v.length === 9) || 'Your Phone Number must be 9 characters long', (v) => !!v || 'Your Phone Number is required', (v) => (v && v.startsWith('7')) || 'Your Phone Number must start with 7', (v) => v !== model.rep_contact1 || 'Phone numbers must be different']"></v-text-field>
                            </div>

                            <!--ABOVE CONTAINERS HAVE BEEN NICED-->

                            
                            <div class="form-column">
                              <v-select xs6 dense :items=sortSub() v-model="model.surbub" label="Suburb/Township" required @change="streetName($event)" :rules="[v => !!v || 'Surbub  is required']" item-text="nom_LOCAL" item-value="cod_LOCAL" class="input-group--focused" style="margin: 1em;"></v-select>
                              <v-text-field xs6 dense name="door_number" type="number" label="Street Number" color="primary"
                                :maxlength="max" v-model="model.door_number" style="margin: 1em;" required @input="validateDoorNumber"></v-text-field>      
                              <v-text-field sx6 dense v-if="custype === 'indi'" name="input-1" label="Next of Kin Firstname" required color="primary" v-model="model.nok_firstname" style="margin: 1em"
                                :rules="[v => /^([a-zA-Z]+((['][a-zA-Z ])?[a-zA-Z]*)){2,30}$/.test(v) || 'Invalid Firstname']"
                              ></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" name="input-1" label="Shareholders Firstname" color="primary" v-model="model.rep_firstname" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders Firstname is required']"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'indi'" v-model="model.nok_contact2" label="Cell Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 110px" @update="onUp1" maxlength="9" :rules="[(v) => !!v || 'Next Of Kin Cell Number is required',]"></v-text-field>
                              <v-text-field sx6 dense v-if="custype === 'org'" v-model="model.rep_contact2" label="Cell Number" required default-country-code="ZW" valid-color="green" style="margin: 1em; width: 110px" @update="onUp1" maxlength="9" :rules="[(v) => !!v || 'Shareholders Cell Number is required',]"></v-text-field>
                            </div>
                          </v-form>


                          <v-form class="form">
                            <div class="form-column">
                              <v-text-field sx6 v-if="custype === 'indi'" name="input-1" label="Next of Kin Physical Address" required color="primary" v-model="model.nok_address" style="margin: 1em" :rules="[(v) => !!v || 'Next Of Kin Address is required',]"></v-text-field>
                              <v-text-field sx6 v-if="custype === 'org'" name="input-1" label="Shareholders Postal Address" color="primary" v-model="model.rep_postal_address" required id="testing" style="margin: 1em" :rules="[(v) => !!v || 'Shareholders Postal Address is required']"></v-text-field>
                            </div>
                          </v-form>


                          <!--DAP ME UP BRA-->
                          <v-form class="form">
                            <v-simple-table height="540px">
                              <template>
                                <thead>
                                  <tr>
                                    <th class="text-left"><font color="black">Type Of Installation</font></th>
                                    <th class="text-left"><font color="black">Number</font></font></th>
                                    <th class="text-left"><font color="black">Number Of Circuits</font></th>
                                    <th class="text-left"><font color="black">Estimatted Wattage</font></th>
                                  </tr>
                                </thead>
                                <tbody>
                                  <tr>
                                    <td style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#3f51b5"><b>Lighting Points</b></font></td>
                                    <td>
                                      <v-text-field name="input-1" type="number" label="Number Required" color="primary"
                                        v-model="model.lighting_points_number" required id="testing" :rules="[v => !!v || 'Cannot be empty']"></v-text-field>
                                    </td>
                                    <td>
                                      <v-text-field name="input-1" label="Number Of Curcuits" type="number" color="primary"
                                        v-model="model.lighting_points_circuits" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                    </td>
                                    <td>
                                      <v-text-field name="input-1" label="Estimated Wattage" type="number" color="primary"
                                        v-model="model.lighting_points_wattage" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                    </td>
                                    </tr>
                                      <tr>
                                        <td style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#3f51b5"><b>Socket outlets</b></font></td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Number Required" color="primary"
                                            v-model="model.socket_outlets_number" required id="testing" :rules="[v => !!v || 'Cannot be empty']"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Number Of Circuits" color="primary"
                                            v-model="model.socket_outlets_circuits" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Estimated Wattage" color="primary"
                                            v-model="model.socket_outlets_wattage" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                      </tr>
                                      <tr>
                                        <td style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#3f51b5"><b>Cooking points</b></font></td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Number Reqiured" color="primary"
                                            v-model="model.cooker_points_number" required id="testing" :rules="[v => !!v || 'Cannot be empty']"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Number Of Circuits" color="primary"
                                            v-model="model.cooker_points_circuits" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Estimated Wattage" color="primary"
                                            v-model="model.cooker_points_wattage" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                      </tr>
                                      <tr>
                                        <td style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#3f51b5"><b>Water heaters</b></font></td>
                                        <td>
                                          <v-text-field name="input-1" label="Number Required" color="primary"
                                            v-model="model.water_heaters_number" required type="number" :rules="[v => !!v || 'Cannot be empty']"
                                            id="testing"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Number Of Circuits" color="primary"
                                            v-model="model.water_heaters_circuits" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Estimated Wattage" color="primary"
                                            v-model="model.water_heaters_wattage" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                      </tr>
                                      <tr>
                                        <td style="font-family: 'Gill Sans', Arial, Helvetica, sans-serif"><font color="#3f51b5"><b>Airconditioning</b></font></td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Number Required" color="primary"
                                            v-model="model.air_conditioning_number" required id="testing" :rules="[v => !!v || 'Cannot be empty']"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Number Of Circuits" color="primary"
                                            v-model="model.air_conditioning_circuits" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                        <td>
                                          <v-text-field name="input-1" type="number" label="Estimated Wattage" color="primary"
                                            v-model="model.air_conditioning_wattage" required id="testing" :rules="[v => !!v || '']"></v-text-field>
                                        </td>
                                      </tr>
                                    </tbody>
                                  </template>
                                </v-simple-table>
                          </v-form>
                        </div>
                      </v-card-text>                      
                    </v-form>
                  </v-card>
                  <v-btn color="primary" @click="nextStep(8)">Continue</v-btn>
                  <v-btn color="green" @click="prevStep(8)">Back</v-btn><br/>
                  <center><v-btn style="margin-top: 20px" outlined big color="red" :href="'/indyconn'">Cancel</v-btn></center>
                </v-stepper-content>
              </v-stepper-items>
            </v-stepper>
          </v-form>
        </v-card-text>
      </v-card>
    </client-only>
  </div>
</template>

<script>
    import API from "@/variables";
    import district from "@/variables/districts";
    import town from "@/variables/towns";
    import surburb from "@/variables/surbubs";
    import document from "@/variables/documents";
    import meter from "@/variables/meter";
    import material from "@/variables/material";
    import duration from "@/variables/duration";
    import purpose from "@/variables/purpose";
    import street from "@/variables/streets";
    import gender from "@/variables/gender";
    import nonstarnd from "@/variables/nonstarnd";
    import reasonforconn from "@/variables/reasonforconn";
    import reasonfordisconn from "@/variables/reasonfordisconn";
    import applicationfor from "@/variables/applicationfor";
    import supplypurpose from "@/variables/supplypurpose";
    import customertype from "@/variables/customertype";
    import existingsupplies from "@/variables/existingsupplies";
    import servpointchang from "@/variables/servpointchang";
    import VueRecaptcha from "vue-recaptcha";
    import VuePhoneNumberInput from "vue-phone-number-input";
    import "vue-phone-number-input/dist/vue-phone-number-input.css";
    import Loading from "vue-loading-overlay";
    import "vue-loading-overlay/dist/vue-loading.css";
    import { upload } from "./file-upload.service";
    import { upload1 } from "./file-uploadpr.service";
    import { upload2 } from "./file-uploadoi.service";
    import { upload3 } from "./file-uploadptr.service";
    import Swal from "sweetalert2";
    import { wait } from "./utils";
    import jsPDF from 'jspdf';
    import html2canvas from 'html2canvas';

    const STATUS_INITIAL  = 0, STATUS_SAVING  = 1, STATUS_SUCCESS  = 2, STATUS_FAILED  = 3, 
            STATUS_INITIAL1 = 0, STATUS_SAVING1 = 1, STATUS_SUCCESS1 = 2, STATUS_FAILED1 = 3,
            STATUS_INITIAL2 = 0, STATUS_SAVING2 = 1, STATUS_SUCCESS2 = 2, STATUS_FAILED2 = 3,
            STATUS_INITIAL3 = 0, STATUS_SAVING3 = 1, STATUS_SUCCESS3 = 2, STATUS_FAILED3 = 3;

    export default {
      layout: "regauth",
      components: {
        VueRecaptcha,
        VuePhoneNumberInput,
        Loading
      },
      data: () => ({
        clickable: false,
        load: false,
        uploadedFiles: [],
        uploadedFiles1: [],
        uploadedFiles2: [],
        uploadedFiles3: [],
        uploadFieldName: "photos",
        uploadFieldName1: "photos",
        uploadFieldName2: "photos",
        uploadFieldName3: "photos",
        uploadError: null,
        uploadError1: null,
        uploadError2: null,
        uploadError3: null,
        currentStatus: null,
        currentStatus1: null,
        currentStatus2: null,
        currentStatus3: null,
        towwns: [],
        ssurbub: [],
        sstreets: [],

        custype: false,
        employed: false,
        rservice: false,
        newaccount: false,

        loading: false,
        visible: false,
        isLoading: false,
        fullPage: true,
        API: API,
        visible1: false,
        errors: [],
        pleaseTickRecaptchaMessage: "",
        recaptchaVerified: false,
        success: [],
        e1: 0,
        max: 12,
        inputField: [v => (v && v.length >= 5) || "Minimum Length is 5"],
        images: null,
        cell: "",
        contractor_cell: "",
        e1: 1,

      model: {
          surname: "",
          cert_of_inc_number: "",
          customer_name: "",
          postal_address_a: "",
          document_id: "",
          email: "",
          owner_contact1: "",
          owner_id_proof: "",
          town: "",
          surbub: "",
          street: "",
          standnumber: "",
          door_number: "",
          duplicator: "",
          owner_contact2: "",
          postal_proof: "",
          connection_type: "",
          cable_length: "",
          main_switch_amp: "",
          main_switch_or: "",      
          main_switch_kva: "",
          emain_switch_amp: "",
          emain_switch_or: "",      
          emain_switch_kva: "",
          cmain_switch_amp: "",
          cmain_switch_or: "",      
          cmain_switch_kva: "",
          c1cable_length: "",
          c3cable_length: "",
          creasons: "",
          servpointchang: "",
          lighting_points_number: "",
          lighting_points_circuits: "",
          lighting_points_wattage: "",
          lighting_points_remarks: "",
          socket_outlets_number: "",
          socket_outlets_circuits: "",
          socket_outlets_wattage: "",
          socket_outlets_remarks: "",
          cooker_points_number: "",
          cooker_points_circuits: "",
          cooker_points_wattage: "",
          cooker_points_remarks: "",
          water_heaters_number: "",
          water_heaters_circuits: "",
          water_heaters_wattage: "",
          water_heaters_remarks: "",
          air_conditioning_number: "",
          air_conditioning_circuits: "",
          air_conditioning_wattage: "",
          air_conditioning_remarks: "",
          other_appliance_number: "",
          other_appliance_circuits: "",
          other_appliance_wattage: "",
          other_appliance_remarks: "",
          motor_phase_number: "",
          motor_phase_circuits: "",
          motor_phase_wattage: "",
          motor_phase_remarks: "",
          motor_phase_number: "",
          motor_phase_circuits: "",
          motor_phase_wattage: "",
          motor_phase_remarks: "",
          motor_rating_number: "",
          motor_rating_circuits: "",
          motor_rating_wattage: "",
          motor_rating_remarks: "",
          motor_plant_number: "",
          motor_plant_circuits: "",
          motor_plant_wattage: "",
          motor_plant_remarks: "",
          contractor_name: "",
          contractor_surname: "",
          contractor_firm: "",
          contractor_address: "",
          contractor_contact1: "",
          contractor_contact2: "",
          nok_id_proof: "",
        },
        duration: duration,
        nonstarnd: nonstarnd,
        reasonforconn: reasonforconn,
        gender: gender,    
        applicationfor: applicationfor,
        supplypurpose: supplypurpose,
        customertype: customertype,
        existingsupplies: existingsupplies,
        material: material,
        servpointchang: servpointchang,
        reasonfordisconn: reasonfordisconn,
        meter: meter,
        purpose: purpose,
        document: document,
        district: district,
        results: {},
        result: {},
      }),
      watch: {        
        custype(newValue) {
          if (newValue === 'indi') {
            this.custype = 'indi';
          } else if (newValue === 'org') {
            this.custype = 'org';
          } else {
            this.custype = null;
          }
        },
        employed(newValue) {
          if (newValue === 'work') {
            this.employed = 'work';
          } else if (newValue === 'home') {
            this.employed = 'home';
          } else {
            this.employed = null;
          }
        },
        rservice(newValue) {
          if (newValue === 'connect') {
            this.rservice = 'connect';
          } else if (newValue === 'disconnect') {
            this.rservice = 'disconnect';
          } else {
            this.rservice = null;
          }
        },
        newaccount(newValue) {
          if (newValue === 'allow') {
            this.newaccount = 'allow';
          } else if (newValue === 'disallow') {
            this.newaccount = 'disallow';
          } else {
            this.newaccount = null;
          }
        }
      },
      methods: {
        onUpdateContact1(value) {
          console.log('First phone number updated:', value)
        },
        onUpdateContact2(value) {
          console.log('Second phone number updated:', value)
        },
        updateModel() {
          this.model.postal_proof = this.uploadedFiles[0].id;
          this.model.owner_id_proof = this.uploadedFiles1[0].id;
          this.model.transfer_add_proof = this.uploadedFiles2[0].id;
          this.model.nok_id_proof = this.uploadedFiles3[0].id;
        },
        validateDoorNumber() {
          if (this.model.door_number < 0) {
           this.model.door_number = 0;
          }
        },
        sortFunc: function () {
          return this.towwns.slice().sort(function (a, b) {
            return (a.nom_MUNIC > b.nom_MUNIC) ? 1 : -1;
          });
        },
        sortSub: function () {
          return this.ssurbub.slice().sort(function (a, b) {
            return (a.nom_LOCAL > b.nom_LOCAL) ? 1 : -1;
          });
        },
        sortStr: function () {
          return this.sstreets.slice().sort(function (a, b) {
            return (a.nom_CALLE > b.nom_CALLE) ? 1 : -1;
          });
        },
        cancelled: function () {
          this.e1 = 1;
          this.$refs.form1.reset();
          this.$refs.form2.reset();
          this.$refs.form3.reset();
          this.$refs.form4.reset();
          this.$refs.form5.reset();
          this.$refs.form6.reset();
          this.$refs.form7.reset();
          this.$refs.form8.reset();
        },
        nextStep: function (e) {
          if (e == 1) {
            this.e1 = e + 1;
          } else if (e == 2) {
            if (this.$refs.form2.validate()) {
              this.e1 = e + 1;
            }
          } else if (e == 3) {
            if (this.$refs.form3.validate()) {
              this.e1 = e + 1;
            }
          }
          else if (e == 4) {
            this.e1 = e + 1;
          }
          else if (e == 5) {
            this.e1 = e + 1;
          }
          else if (e == 6) {
            if (this.$refs.form6.validate()) {
              this.e1 = e + 1;
            }
          }
          else if (e == 7) {
            if (this.$refs.form7.validate()) {
              this.e1 = e + 1;
            }
          }
          else if (e == 8) {
            if (this.$refs.form8.validate()) {
              this.e1 = e + 1;
            }
          }
        },
        onUpdate(payload) {
          this.results = payload;
        },
        onUp(payload) {
          this.result = payload;
        },
        onUpdate1(payload) {
          this.results1 = payload;
        },
        onUp1(payload) {
          this.result1 = payload;
        },
        onUpdate2(payload) {
          this.results2 = payload;
        },
        onUp2(payload) {
          this.result2 = payload;
        },
        onUpdate3(payload) {
          this.results3 = payload;
        },
        onUp3(payload) {
          this.result3 = payload;
        },
        prevStep: function (e) {
          if (e == 2) {
            this.e1 = e - 1;
          } else if (e == 3) {
            this.e1 = e - 1;
          } else if (e == 4) {
            this.e1 = e - 1;
          } else if (e == 5) {
            this.e1 = e - 2;
          } else if (e == 6) {
            this.e1 = e - 1;
          } else if (e == 7) {
            this.e1 = e - 1;
          } else if (e == 8) {
            this.e1 = e - 1;
          }
        },
        surbubName(event) {
          this.$axios
          .request({
            url: "http://172.16.13.12:5050/selfservice_test/job/quotation/suburbs/" + this.model.town,
            method: "post",
            baseURL: process.env.baseUrl,
          })
          .then(response => {
            this.ssurbub = response.data
            console.log(response.data);
          })
          .catch(e => {
            console.log(e);
          });
        },
        streetName(event) {
          this.$axios
          .request({
            url: "http://172.16.13.12:5050/selfservice_test/job/quotation/streets/" + this.model.surbub,
            method: "post",
            baseURL: process.env.baseUrl,
          })
          .then(response => {
            this.sstreets = response.data
            console.log(response.data);
          })
          .catch(e => {
            console.log(e);
          });
        },    
        register() {
          this.errors = [];
          this.success = [];        
          if (this.$refs.form2.validate() & this.$refs.form3.validate() & this.$refs.form5.validate() & this.$refs.form6.validate() & this.$refs.form7.validate()) {
            if (this.results.isValid == true) {
              this.model.customer_name = this.model.customer_name.toUpperCase();
              this.model.surname = this.model.surname.toUpperCase();
              this.model.document_id = this.model.document_id.toUpperCase();
              this.model.postal_address_a = this.model.postal_address_a.toUpperCase();
              this.model.gender = this.model.gender.toUpperCase();
              this.model.contractor_name = this.model.contractor_name.toUpperCase();
              this.model.contractor_surname = this.model.contractor_surname.toUpperCase();
              this.model.contractor_firm = this.model.contractor_firm.toUpperCase();          
              this.model.contractor_address = this.model.contractor_address.toUpperCase();          
              this.isLoading = true;
              this.$axios.request({
                url: "http://172.16.4.1:8182/api/newnetmet7",
                method: "post", 
                headers:{'Authorization': 'Bearer '+localStorage.getItem('token')},
                data: this.model,
              })
              .then((response) => {
                this.isLoading = false;
                if (response.data != "" || response.data != null) {
                  this.success.push( "Application successful. Please note, your PJOB number is " + response.data);
                  this.$router.push({ path: "/success1", query: { data: response.data },});
                } else {
                    this.showMessage();
                  }
              }).catch((e) => {
                this.isLoading = false;
                this.showMessage();
              });
            } 
            else {
              this.isLoading = false;
              this.showMessage();              
            }
          }
          else {
            if (!this.$refs.form2.validate()) {
              this.e1 = 2;
            } else if (!this.$refs.form3.validate()) {
              this.e1 = 3;
            } else if (!this.$refs.form4.validate()) {
              this.e1 = 4;
            } else if (!this.$refs.form5.validate()) {
              this.e1 = 5;
            } else if (!this.$refs.form6.validate()) {
              this.e1 = 6;
            } else if (!this.$refs.form7.validate()) {
              this.e1 = 7;
            }
            this.isLoading = false;
          }
        },
        reset() {
          this.currentStatus = STATUS_INITIAL;
          this.uploadedFiles = [];
          this.uploadError = null;
        },
        reset1() {
          this.currentStatus1 = STATUS_INITIAL1;
          this.uploadedFiles1 = [];
          this.uploadError1 = null;
        },
        reset2() {
          this.currentStatus2 = STATUS_INITIAL2;
          this.uploadedFiles2 = [];
          this.uploadError2 = null;
        },
        reset3() {
          this.currentStatus3 = STATUS_INITIAL3;
          this.uploadedFiles3 = [];
          this.uploadError3 = null;
        },
        resetme() {
          this.currentStatus = STATUS_INITIAL;
          this.uploadedFiles = [];
          this.uploadError = null;
        },
        resetme1() {
          this.currentStatus1 = STATUS_INITIAL1;
          this.uploadedFiles1 = [];
          this.uploadError1 = null;
        },
        resetme2() {
          this.currentStatus2 = STATUS_INITIAL2;
          this.uploadedFiles2 = [];
          this.uploadError2 = null;
        },
        resetme3() {
          this.currentStatus3 = STATUS_INITIAL3;
          this.uploadedFiles3 = [];
          this.uploadError3 = null;
        },      
        save(formData) {
          this.currentStatus = STATUS_SAVING;
          upload(formData)
          .then(wait(1500))
          .then((x) => {
            this.uploadedFiles = [].concat(x);
            this.currentStatus = STATUS_SUCCESS;
          })
          .catch((err) => {
            this.uploadError = err.response;
            this.currentStatus = STATUS_FAILED;
          });
        },
        filesChange(fieldName, fileList) {
          const formData = new FormData();
          if (!fileList.length) return;
          Array.from(Array(fileList.length).keys()).map((x) => {
            formData.append(fieldName, fileList[x], fileList[x].name);
          });
          this.save(formData);
        },
        save1(formData1) {
          this.currentStatus1 = STATUS_SAVING1;
          upload1(formData1)
          .then(wait(1500))
          .then((x) => {
            this.uploadedFiles1 = [].concat(x);
            this.currentStatus1 = STATUS_SUCCESS1;
          })
          .catch((err) => {
            this.uploadError1 = err.response;
            this.currentStatus1 = STATUS_FAILED1;
          });
        },
        filesChange1(fieldName, fileList) {
          const formData1 = new FormData();
          if (!fileList.length) return;
          Array.from(Array(fileList.length).keys()).map((x) => {
            formData1.append(fieldName, fileList[x], fileList[x].name);
          });
          this.save1(formData1);
        },
        save2(formData2) {
          this.currentStatus2 = STATUS_SAVING2;
          upload2(formData2)
          .then(wait(1500))
          .then((x) => {
            this.uploadedFiles2 = [].concat(x);
            this.currentStatus2 = STATUS_SUCCESS2;
          })
          .catch((err) => {
            this.uploadError2 = err.response;
            this.currentStatus2 = STATUS_FAILED2;
          });
        },
        filesChange2(fieldName, fileList) {
          const formData2 = new FormData();
          if (!fileList.length) return;
          Array.from(Array(fileList.length).keys()).map((x) => {
            formData2.append(fieldName, fileList[x], fileList[x].name);
          });
          this.save2(formData2);
        },
        save3(formData3) {
          this.currentStatus3 = STATUS_SAVING3;
          upload3(formData3)
          .then(wait(1500))
          .then((x) => {
            this.uploadedFiles3 = [].concat(x);
            this.currentStatus3 = STATUS_SUCCESS3;
          })
          .catch((err) => {
            this.uploadError3 = err.response;
            this.currentStatus3 = STATUS_FAILED3;
          });
        },
        filesChange3(fieldName, fileList) {
          const formData3 = new FormData();
          if (!fileList.length) return;
          Array.from(Array(fileList.length).keys()).map((x) => {
            formData3.append(fieldName, fileList[x], fileList[x].name);
          });
          this.save3(formData3);
        }
      },
      created() {
        this.$axios
          .request({
            url: "http://172.16.13.12:5050/selfservice_test/job/quotation/towns",
            method: "post",
            crossDomain: true,
            baseURL: process.env.baseUrl,
          })
          .then(response => {
            this.towwns = response.data
            console.log(response.data);
          })
          .catch(e => {
            console.log(e);
          });
      },
      computed: {
        uploadmodel() {
          return {
            postal_proof: this.uploadedFiles[0].id,
            owner_id_proof: this.uploadedFiles1[0].id,
            transfer_add_proof: this.uploadedFiles2[0].id,
            nok_id_proof: this.uploadedFiles3[0].id,
          };
        },
        computeLogo() {
          return "/logo.png";
        },
        isInitial() {
          return this.currentStatus === STATUS_INITIAL;
        },
        isSaving() {
          return this.currentStatus === STATUS_SAVING;
        },
        isSuccess() {
          return this.currentStatus === STATUS_SUCCESS;
        },
        isFailed() {
          return this.currentStatus === STATUS_FAILED;
        },
        isInitial1() {
          return this.currentStatus1 === STATUS_INITIAL1;
        },
        isSaving1() {
          return this.currentStatus1 === STATUS_SAVING1;
        },
        isSuccess1() {
          return this.currentStatus1 === STATUS_SUCCESS1;
        },
        isFailed1() {
          return this.currentStatus1 === STATUS_FAILED1;
        },
        isInitial2() {
          return this.currentStatus2 === STATUS_INITIAL2;
        },
        isSaving2() {
          return this.currentStatus2 === STATUS_SAVING2;
        },
        isSuccess2() {
          return this.currentStatus2 === STATUS_SUCCESS2;
        },
        isFailed2() {
          return this.currentStatus2 === STATUS_FAILED2;
        },
        isInitial3() {
          return this.currentStatus3 === STATUS_INITIAL3;
        },
        isSaving3() {
          return this.currentStatus3 === STATUS_SAVING3;
        },
        isSuccess3() {
          return this.currentStatus3 === STATUS_SUCCESS3;
        },
        isFailed3() {
          return this.currentStatus3 === STATUS_FAILED3;
        },
      },
      mounted() {
        this.reset();
        this.reset1();
        this.reset2();
        this.reset3();
        this.e1 = 1;
      },
    };
</script>

<style scoped lang="css">
  h1 {
    font-size: 28px !important;
  }

  .print-content {
    /* Styles for the print content */
  }

  .layout {
    margin-top: 30px !important;
  }

  .inputPrice input[type="number"] {
    appearance: textfield;
    -moz-appearance: textfield;
  }

  .inputPrice input::-webkit-outer-spin-button,
  .inputPrice input::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }

  #pageDashboard {
    overflow: auto;
  }

  .dropbox {
    outline: 2px dashed grey;
    outline-offset: -10px;
    background: lightcyan;
    color: dimgray;
    max-height: 120px;
    position: relative;
    cursor: pointer;
  }

  .input-file {
    opacity: 0;
    width: 80%;
    height: 50px;
    position: absolute;
    cursor: pointer;
  }

  .dropbox:hover {
    background: lightblue;
  }

  .dropbox p {
    font-size: 0.8em;
    text-align: center;
    padding: 10px 0;
  }

  h1 {
    font-size: 28px !important;
  }

  .layout {
    margin-top: 30px !important;
  }

  .inputPrice input[type="number"] {
    appearance: textfield;
    -moz-appearance: textfield;
  }

  .inputPrice input::-webkit-outer-spin-button,
  .inputPrice input::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }

  #pageDashboard {
    overflow: auto;
  }

  .text-field-transparent .v-input__slot {
    opacity: 0.5;
  }

  .unclickable {
    pointer-events: none;
  }
  .form-column {
    flex: 1;
    margin-right: 20px;
  }

  .form {
    display: flex;
  }

  .reduced-label-font {
    font-size: 4px;
  }
</style>

