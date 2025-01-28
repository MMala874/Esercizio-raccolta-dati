<script>
    let formData = {
      name: '',
      age: null,
      presentation: '',
      country: '',
      companies: [],
      remoteWork: false,
      experience: ''
    };
  
    let currentStep = 1;
    const totalSteps = 7;
  
    function validateStep(step) {
      switch(step) {
        case 1: return formData.name.trim() !== '';
        case 2: return formData.age !== null && formData.age > 0;
        case 3: return formData.presentation.trim() !== '';
        case 4: return formData.country !== '';
        case 5: return formData.companies.length > 0;
        case 6: return true;
        case 7: return formData.experience !== '';
        default: return false;
      }
    }
  
    function nextStep() {
      if (validateStep(currentStep)) {
        if (currentStep < totalSteps) {
          currentStep++;
        }
      } else {
        alert('Per favore, compila tutti i campi richiesti');
      }
    }
  
    function prevStep() {
      if (currentStep > 1) {
        currentStep--;
      }
    }
  
    function submitForm() {
      if (validateStep(7)) {
        formData = {
          name: '',
          age: null,
          presentation: '',
          country: '',
          companies: [],
          remoteWork: false,
          experience: ''
        };
        currentStep = 1;
        alert('Grazie! Il modulo è stato inviato e resetato.');
      }
    }
  
    function toggleCompany(company) {
      const index = formData.companies.indexOf(company);
      if (index > -1) {
        formData.companies.splice(index, 1);
      } else {
        formData.companies.push(company);
      }
      formData.companies = formData.companies;
    }
  </script>
  
  <div class="form-container">
    <h2>Modulo di Registrazione (Passaggio {currentStep} di {totalSteps})</h2>
    
    {#if currentStep === 1}
      <div class="form-step">
        <label for="name">Qual è il tuo nome?</label>
        <input 
          type="text" 
          id="name" 
          bind:value={formData.name} 
          placeholder="Inserisci il tuo nome"
        />
      </div>
    {/if}
  
    {#if currentStep === 2}
      <div class="form-step">
        <label for="age">Quanti anni hai?</label>
        <input 
          type="number" 
          id="age" 
          bind:value={formData.age} 
          min="0" 
          placeholder="Inserisci la tua età"
        />
      </div>
    {/if}
  
    {#if currentStep === 3}
      <div class="form-step">
        <label for="presentation">Scrivi una breve presentazione su di te</label>
        <textarea 
          id="presentation" 
          bind:value={formData.presentation} 
          placeholder="Presentati brevemente..."
        ></textarea>
      </div>
    {/if}
  
    {#if currentStep === 4}
      <div class="form-step">
        <label for="country">In quale paese vivi?</label>
        <select bind:value={formData.country}>
          <option value="">Seleziona un paese</option>
          <option value="Italia">Italia</option>
          <option value="Francia">Francia</option>
          <option value="Spagna">Spagna</option>
          <option value="Germania">Germania</option>
          <option value="Altro">Altro</option>
        </select>
      </div>
    {/if}
  
    {#if currentStep === 5}
      <div class="form-step">
        <label>In quali aziende hai lavorato?</label>
        <div class="checkbox-group">
          {#each ['Google', 'Microsoft', 'Amazon', 'Altro'] as company}
            <label>
              <input 
                type="checkbox" 
                value={company}
                checked={formData.companies.includes(company)}
                on:change={() => toggleCompany(company)}
              /> {company}
            </label>
          {/each}
        </div>
      </div>
    {/if}
  
    {#if currentStep === 6}
      <div class="form-step">
        <label>
          <input 
            type="checkbox" 
            bind:checked={formData.remoteWork}
          /> Sei disponibile a lavorare da remoto?
        </label>
      </div>
    {/if}
  
    {#if currentStep === 7}
      <div class="form-step">
        <label>Quanti anni di esperienza lavorativa hai?</label>
        <div class="radio-group">
          {#each ['Nessuna', '1-2 anni', '3-5 anni', 'Più di 5 anni'] as option}
            <label>
              <input 
                type="radio" 
                name="experience" 
                value={option}
                checked={formData.experience === option}
                on:change={() => formData.experience = option}
              /> {option}
            </label>
          {/each}
        </div>
      </div>
    {/if}
  
    <div class="navigation-buttons">
      {#if currentStep > 1}
        <button on:click={prevStep}>Precedente</button>
      {/if}
      
      {#if currentStep < totalSteps}
        <button on:click={nextStep}>Successivo</button>
      {:else}
        <button on:click={submitForm}>Invia</button>
      {/if}
    </div>
  </div>
  
  <style>
    .form-container {
      max-width: 500px;
      margin: 0 auto;
      padding: 20px;
      text-align: center;
    }
  
    .form-step {
      display: flex;
      flex-direction: column;
      margin-bottom: 20px;
    }
  
    label {
      margin-bottom: 10px;
    }
  
    input, select, textarea {
      padding: 10px;
      margin-bottom: 10px;
    }
  
    .checkbox-group, .radio-group {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
    }
  
    .navigation-buttons {
      display: flex;
      justify-content: space-between;
    }
  </style>