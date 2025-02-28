<script lang="ts">
    import { goto } from '$app/navigation';
  
    let form = {
  name: '',
  email: '',
  address: '',
  phone: '',
  aadhaar: '', 
  role: '',  // New field for Aadhaar number
  passportPhoto: null as string | null,  // Allow both string and null
  staffPhotos: [] as string[]            // Array of strings for image URLs
};
  
    let isSubmitting = false;
  
    const handleSubmit = () => {
      if (isSubmitting) return;
      isSubmitting = true;
  
      if (
  form.name.trim() &&
  form.email.trim() &&
  form.address.trim() &&
  form.phone.trim() &&
  form.aadhaar.trim() &&  // Aadhaar validation
  form.passportPhoto &&
  form.role // Role validation
) {
  localStorage.setItem('userDetails', JSON.stringify(form));
  goto('/details');
} else {
  alert('Please fill in all fields including Aadhaar number and upload a passport-size photo.');
  isSubmitting = false;
}

 // Aadhaar Number Validation
  const isValidAadhaar = /^[0-9]{12}$/.test(form.aadhaar);
  if (!isValidAadhaar) {
    alert('Please enter a valid 12-digit Aadhaar number.');
    isSubmitting = false;
    return;  // This return is now correctly inside the function
  }

    };
  
    const handlePassportPhoto = (event: Event) => {
      const file = (event.target as HTMLInputElement).files?.[0];
      if (file) {
        form.passportPhoto = URL.createObjectURL(file);
      }
    };
  
    const handleStaffPhotos = (event: Event) => {
      const files = (event.target as HTMLInputElement).files;
      if (files) {
        form.staffPhotos = Array.from(files).map(file => URL.createObjectURL(file));
      }
    };

    
   
  </script>
  
  <main class="container">
    <!-- Role Selection -->


    <!-- Animated Gradient Background -->
    <div class="bg-gradient-animation"></div>
    
  
    <!-- Left Side Form -->
    <div class="left-section">
      <h2>User Details</h2>
  
      <input
        type="text"
        placeholder="Full Name"
        bind:value={form.name}
        class="input-field"
      />
      <input
        type="email"
        placeholder="Email Address"
        bind:value={form.email}
        class="input-field"
      />
      <input
        type="text"
        placeholder="Address"
        bind:value={form.address}
        class="input-field"
      />
      <input
        type="text"
        placeholder="Phone Number"
        bind:value={form.phone}
        class="input-field"
      />

      <input
  type="text"
  placeholder="Aadhaar Number"
  bind:value={form.aadhaar}
  class="input-field"
  maxlength="12" 
/>

<label class="label">Select Role:</label>
<div class="role-selection">
  <label>
    <input
      type="checkbox"
      bind:group={form.role}
      value="Student"
    /> Student
  </label>
  <label>
    <input
      type="checkbox"
      bind:group={form.role}
      value="Police Officer"
    /> Police Officer
  </label>
</div>

  
      <!-- Passport-size Photo Upload -->
      <label class="label">Passport-size Photo:</label>
      <input type="file" accept="image/*" on:change={handlePassportPhoto} class="file-input" />
  
      <!-- Police Staff Photos Upload -->
      <label class="label">Police Staff Photos:</label>
      <input type="file" accept="image/*" multiple on:change={handleStaffPhotos} class="file-input" />
      
      <div class="staff-photos">
        {#each form.staffPhotos as photo}
          <img src={photo} alt="Staff Photo" class="preview-image" />
        {/each}
      </div>
  
    </div>
    <button on:click={handleSubmit} class="submit-button">Submit</button>
  
    <!-- Top-Right Passport Photo Preview -->
    {#if form.passportPhoto}
      <img src={form.passportPhoto} alt="Passport Photo" class="top-right-passport" />
    {/if}

    <!-- Logo at the Top-Left Corner -->
    <img src="/logo/police.png" alt="Logo" class="logo">
    <hr class="logo-border">
    <h1 class="heading">DSP 27 Library</h1>

   <!-- Top-Right Passport Photo Preview -->
<div class="top-right-passport-container">
    {#if form.passportPhoto}
      <img src={form.passportPhoto} alt="Passport Photo" class="top-right-passport" />
    {:else}
      <div class="placeholder-text">Image Preview Here</div>
    {/if}
  </div>
  
  </main>
  
  <style>
    /* Container for Top-Right Passport Photo */
.top-right-passport-container {
  position: fixed;
  top: 272px;
  right: 105px;
  width: 137px;
  height: 169px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 0.1px dashed white;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgb(0 0 0 / 17%);
  background: rgb(243 243 243 / 10%);
  color: white;
  text-align: center;
  font-size: 0.9rem;
  font-style: italic;
}

/* Hide border when image is uploaded */
.top-right-passport-container img {
  border: none;
}

/* Placeholder Text */
.placeholder-text {
  color: white;
  opacity: 0.7;
}

   .heading {
  position: fixed;
  top: 41px;
  left: 68vh; /* Adjust this to align it perfectly */
  color: white;
  font-size: 3.5rem;
  font-weight: bold;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  z-index: 10;
}


    .logo-border {
  position: fixed;
  top: 186px; /* Adjust this to align the line perfectly */
  /* left: 54px; */
  width: 100%;
  border: none;
  border-bottom: 2px solid white;
}

    .role-selection {
  display: flex;
  gap: 20px;
  margin-bottom: 10px;
}

.role-selection label {
  font-size: 1rem;
  color: white;
}

    /* Logo Styling */
.logo {
  position: fixed;
  top: 40px;
  left: 54px;
  width: 100px;
  height: auto;
  z-index: 10;
  
}

    /* Container and Background */
    .container {
      min-height: 98vh;
      display: flex;
      align-items: flex-start;
      justify-content: flex-start;
      position: relative;
      overflow: hidden;
    }
  
    .bg-gradient-animation {
      position: absolute;
      inset: 0;
      background: linear-gradient(45deg, #1d0000, #054248, #382e48, #2a3151, #414109);
      background-size: 400% 400%;
      animation: gradientAnimation 15s ease infinite;
      z-index: -1;
    }
  
    @keyframes gradientAnimation {
      0% {
        background-position: 0% 50%;
      }
      50% {
        background-position: 100% 50%;
      }
      100% {
        background-position: 0% 50%;
      }
    }
  
    /* Left Section Styling */
    .left-section {
      padding: 2rem;
      color: white;
      display: flex;
      flex-direction: column;
      gap: 1rem;
      margin-left: 1vh;
    margin-top:148px;
    }
  
    .left-section h2 {
      font-size: 2rem;
    }
  
    .input-field {
      width: 300px;
      padding: 0.75rem;
      border: none;
      border-radius: 10px;
      background: rgba(255, 255, 255, 0.8);
      box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
      outline: none;
    }
  
    .file-input {
      margin: 10px 0;
    }
  
    .label {
      font-size: 1rem;
      font-weight: bold;
    }
  
    .staff-photos {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }
  
    .preview-image {
      width: 80px;
      height: 80px;
      object-fit: cover;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
      border: 2px solid white;
    }
  
    .submit-button {
      width: 150px;
      padding: 0.75rem;
      border: none;
      border-radius: 10px;
      background: linear-gradient(to right, #ff6b6b, #845ec2);
      color: white;
      font-weight: bold;
      cursor: pointer;
      position: absolute;
    bottom: 41px;
    right: 9vh;
    }
  
    /* Top-Right Passport Photo */
    .top-right-passport {
      position: fixed;
      top: 272px;
      right: 105px;
      width: 137px;
      height: 169px;
      object-fit: cover;
      border-radius: 8px;
      /* box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3); */
      border: 2px solid white;
    }
  </style>
  