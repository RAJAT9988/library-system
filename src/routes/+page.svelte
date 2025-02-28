<script lang="ts">
  import { goto } from '$app/navigation';
  import type { SignupForm } from '../lib/types';

  let form: SignupForm = {
    name: '',
    email: '',
    password: ''
  };

  let isSubmitting = false;
  

const handleSubmit = () => {
if (isSubmitting) return; // Prevent multiple clicks
isSubmitting = true;

if (form.name.trim() && form.email.trim() && form.password.trim()) {
  localStorage.setItem('user', JSON.stringify(form));
  goto('/details');
} else {
  alert('Please fill in all fields.');
  isSubmitting = false;
}
};

</script>

<main class="container">
  <!-- Animated Gradient Background -->
  <div class="bg-gradient-animation"></div>

  <!-- Signup Card with Enhanced Glassmorphism Effect -->
  <div class="signup-card">
    <h2>Create an Account</h2>

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
      type="password"
      placeholder="Password"
      bind:value={form.password}
      class="input-field"
    />

    <button
      on:click={handleSubmit}
      class="signup-button"
    >
      Sign Up
    </button>

    <p class="login-link">
      Already have an account?
      <a href="/login">Log In</a>
    </p>
  </div>
</main>

<style>
  /* Container and Background */
  .container {
    min-height: 98vh;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }

  .bg-gradient-animation {
    position: absolute;
    inset: 0;
    background: linear-gradient(45deg,#7f741d,#070101,#fdfdfd,#9a28ce,#f5f5f5,#b1995e,#000000);
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

  /* Signup Card */
  .signup-card {
    backdrop-filter: blur(15px);
    background: rgba(255, 255, 255, 0.15);
    border: 1px solid rgba(255, 255, 255, 0.3);
    padding: 2rem;
    border-radius: 20px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
    width: 300px;
    text-align: center;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .signup-card:hover {
    transform: scale(1.05);
    box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
  }

  .signup-card h2 {
    font-size: 2rem;
    color: white;
    margin-bottom: 1.5rem;
  }

  .input-field {
    width: 92%;
    padding: 0.75rem;
    margin-bottom: 1rem;
    border: none;
    border-radius: 10px;
    background: rgba(255, 255, 255, 0.8);
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
    outline: none;
    transition: box-shadow 0.3s ease;
  }

  .input-field:focus {
    box-shadow: 0 0 8px rgba(100, 60, 200, 0.6);
  }

  .signup-button {
    width: 100%;
    padding: 0.75rem;
    border: none;
    border-radius: 10px;
    background: linear-gradient(to right, #ff6b6b, #845ec2);
    color: white;
    font-weight: bold;
    cursor: pointer;
    transition: background 0.3s ease, box-shadow 0.3s ease;
  }

  .signup-button:hover {
    background: linear-gradient(to right, #845ec2, #ff6b6b);
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  }

  .login-link {
    margin-top: 1rem;
    font-size: 0.85rem;
    color: rgba(255, 255, 255, 0.85);
  }

  .login-link a {
    color: #ffc75f;
    text-decoration: none;
    transition: color 0.3s ease;
  }

  .login-link a:hover {
    color: #f9f871;
  }
</style>
sdfghjhgfdsasdfhjkhg