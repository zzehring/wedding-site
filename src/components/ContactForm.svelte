<script>
  let name = '';
  let email = '';
  let phone = '';
  let status = ''; // 'submitting', 'success', 'error'

  async function handleSubmit() {
    status = 'submitting';
    
    // Simulate a network request
    console.log('Submitting:', { name, email, phone });

    // This is where you'll call your serverless function
    // For now, we'll just simulate success
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    // Once we have an endpoint, we can do something like:
    /*
    try {
      const response = await fetch('/api/submit-contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name, email, phone }),
      });

      if (response.ok) {
        status = 'success';
        name = '';
        email = '';
        phone = '';
      } else {
        status = 'error';
      }
    } catch (error) {
      console.error('Submission error:', error);
      status = 'error';
    }
    */
   
    status = 'success'; // Remove this once you have a real endpoint
  }
</script>

<style>
  h2 {
    text-align: center;
    margin-top: 0;
  }
  .contact-form {
    max-width: 500px;
    margin: 2rem auto;
    padding: 2rem;
    background-color: #f9f9f9;
    border-radius: 8px;
    border: 1px solid #ddd;
  }
  .form-group {
    margin-bottom: 1rem;
  }
  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 600;
  }
  input {
    width: 100%;
    padding: 0.75rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 1rem;
    box-sizing: border-box;
  }
  button {
    width: 100%;
    padding: 0.75rem;
    background-color: #4A442D;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  button:hover {
    background-color: #6E8483;
  }
  button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .status-message {
    text-align: center;
    padding: 1rem;
    border-radius: 4px;
    margin-top: 1rem;
  }
  .success {
    background-color: #d4edda;
    color: #155724;
  }
  .error {
    background-color: #f8d7da;
    color: #721c24;
  }
</style>

<div class="contact-form">
  <h2>Stay in Touch</h2>
  <p>Please provide your name and email so we can send you updates and your formal invitation.</p>
  {#if status === 'success'}
    <div class="status-message success">
      Thank you! We've received your information.
    </div>
  {:else}
    <form on:submit|preventDefault={handleSubmit}>
      <div class="form-group">
        <label for="name">Name</label>
        <input id="name" type="text" bind:value={name} required disabled={status === 'submitting'} />
      </div>
      <div class="form-group">
        <label for="email">Email</label>
        <input id="email" type="email" bind:value={email} required disabled={status === 'submitting'} />
      </div>
      <div class="form-group">
        <label for="phone">Phone Number</label>
        <input id="phone" type="tel" bind:value={phone} required disabled={status === 'submitting'} />
      </div>
      <button type="submit" disabled={status === 'submitting'}>
        {#if status === 'submitting'}
          Submitting...
        {:else}
          Submit
        {/if}
      </button>
    </form>
    {#if status === 'error'}
      <div class="status-message error">
        Something went wrong. Please try again.
      </div>
    {/if}
  {/if}
</div> 