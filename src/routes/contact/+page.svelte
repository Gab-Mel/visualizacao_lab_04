<svelte:head>
  <title>Contact me</title>
</svelte:head>
<h1>
    Contatos
</h1>
<form action="mailto:gabrielzinho.melo.10@gmail.com" method="GET">
    <label for="email">Email:</label> <br>
    <input name="email" type="email"> <br>
    <label for="subject">Subject:</label> <br>
    <input name="subject" type="text"> <br>
    <label for="body">Body:</label> <br>
    <textarea name="body"></textarea> <br>
    <button>Submit</button>
</form>