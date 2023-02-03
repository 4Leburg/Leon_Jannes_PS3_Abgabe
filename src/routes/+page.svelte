<script>

    import Navbar from '$lib/components/Navbar.svelte';
    
    
    </script>
    
    <div class="grid-item-4" style="height: 90px;">
        <div class="content">
        <Navbar> </Navbar>
        <div class="rectangle"></div>
      </div>
    </div>

    <style>
    .rectangle {
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        height: 90px;
        background-color: black;
    }
    
    </style>