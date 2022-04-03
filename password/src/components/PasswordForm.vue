<template>
 <form>
    <ul>
			<li v-bind:class="{ is_valid: contains_number }">Contains Number</li>
			<li v-bind:class="{ is_valid: contains_uppercase }">Contains Uppercase</li>
      <li v-bind:class="{ is_valid: contains_lowcase }">Contains LowCase</li>
			<li v-bind:class="{ is_valid: contains_special_character }">Contains Special Character</li>
		</ul>

    <label>Password: </label>
    <input type="password" @input="checkPassword" v-model="password"/>

    <label>Confirm Password: </label>
    <input type="password" v-model="confirmPassword"/> 

    <div class="message" v-if="passwordError">{{ passwordError }} </div>
    <div class="message" v-if="security">{{ security }} </div>

    <div class="submit">
      <button @click.prevent="handleSubmit">Validate</button>
    </div>
            
    <div class="submit">
      <button class="btn-refresh" @click.prevent="generatedPassword">Generate a Password</button>
    </div>
  </form>
</template>

<script>

  export default {
    data() {
      return {
        password: "",
        confirmPassword: "",
        passwordError: "",
        contains_number: false,
        contains_uppercase: false,
        contains_lowcase: false,
        contains_special_character: false,
        valid_password: false,
        security: "",
        generatePassword: "",
        characters: "abcdefghijklmnopqrstuvwxyz1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZ!@#$%^&*?",
      }
    },

    methods: {

    //Verifie lors de la validation si les 2 champs sont bien identiques et contiennent au moins 8 caratères
      handleSubmit() {
        if(this.password === this.confirmPassword) {
          if(this.password.length < 7){
            this.passwordError =  "Le mot de passe doit faire au moins 8 caratères"
          } else {
            this.passwordError = "Votre mot de passe a été enregistré"
            this.security = ""
          }         
        } else {
          this.passwordError = "Les mots de passe ne sont pas identiques"
        }
      },

    // Vérifie si le champ Input contient bien une majuscule, une minuscule, un nombre et un caratère special
      checkPassword() {
			const format = /[ !@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]/; 
        
        this.contains_number = /\d/.test(this.password);
        this.contains_uppercase = /[A-Z]/.test(this.password);
        this.contains_lowcase = /[a-z]/.test(this.password);
        this.contains_special_character = format.test(this.password);

    // Affiche un message en fonction de la puissance du mot de passe
        if(this.contains_number && this.contains_uppercase && this.contains_lowcase && this.contains_special_character == true) {
            this.security = "Mot de passe Très Fort"
        } else if((this.contains_number && this.contains_uppercase || this.contains_number && this.contains_lowcase || this.contains_uppercase && this.contains_lowcase)  == true && this.contains_special_character == false) {
            this.security = "Mot de passe Moyen"
        } else if(this.contains_number && this.contains_special_character || this.contains_lowcase && this.contains_special_character || this.contains_uppercase && this.contains_special_character == true) {
            this.security = "Mot de passe Fort"
        } else {
          this.security = "Mot de passe faible"
        }
      },

    // Génère un mot de passe Aléatoire
      generatedPassword() {
        this.generatePassword = ""
        this.security = ""
        for (let i = 0; i < 10; i++) {
          this.generatePassword += this.characters.charAt(
          Math.floor(Math.random() * this.characters.length)
          );
        }
        
        this.password = this.generatePassword
        this.confirmPassword = this.password
      },
    },
};
</script>

<style>
  form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }

  label {
    color:#aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6em;
    text-transform: uppercase;
    font-weight: bold;
  }

  input {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border-bottom: 1px solid #ddd;
    border: none;
    color: #555;
  }

  button {
    background:green;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
  }

  .submit {
    text-align: center;
  }

  ul {
	padding-left: 20px;
	display: flex;
	flex-direction: column;
	align-items: flex-start;
  }

  li { 
    margin-bottom: 8px;
    color: #525f7f;
    position: relative;
  }

  li:before {
    content: "";
    width: 0%; height: 2px;
    background: #2ecc71;
    position: absolute;
    left: 0; top: 50%;
    display: block;
    transition: all .6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }

  .is_valid { color: rgba(136, 152, 170, 0.8); }
  .is_valid:before { width: 100%; }
  
  .message {
    padding: 1.5em 0;
  }
  

</style>