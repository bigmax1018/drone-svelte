<script>
  import { onMount } from "svelte";
  import { user } from "../js/user.js";
  import { getDocuments } from "../js/lotteries.js";
  import { get_user, user_regist } from "../js/profile.js";
  import { 
    APPWRITE_FOOD_COLLECTION_ID,
    FOOD_TAG_POPULAR,
    FOOD_TAG_FEATURED,
    APPWRITE_USRE_LIST_COLLECTION_ID,
   } from "../js/constants.js";

  export let f7router;


  import { user_name, user_email, account_type, currentMenuItem } from "../js/store";

  let uname = '';
  let uemail = '';
  user_name.subscribe(value => uname = value);
  user_email.subscribe(value => uemail = value);

  let foodItems = [];
  let user_data;
  let user_type;

  onMount(async () => {

    foodItems = (await getDocuments(APPWRITE_FOOD_COLLECTION_ID)).documents;
    user_data = await get_user(APPWRITE_USRE_LIST_COLLECTION_ID, $user.$id);
    if(user_data.total > 0){
      user_type = user_data['documents'][0].user_type;
      if(user_type == 0){
        currentMenuItem.set("My Order");
        account_type.set("customer");
      }else if (user_type == 1){
        currentMenuItem.set("My Store");
        account_type.set("owner");
        f7router.navigate('/home-store/'); 
      }
    }else{
      const data = {
					user_id: $user.$id,
					user_type: 0,
      };
      try {
        await user_regist(APPWRITE_USRE_LIST_COLLECTION_ID, data);
      } catch (exception) {	
        console.error(": ", exception);
      }
      currentMenuItem.set("My Order");
      account_type.set("customer");
    }
  });

</script>

<div class="page page-home">
  <!-- Page Content -->
  <div class="page-content content-area pt-40 pb-0">
    <!-- Search backdrop -->
    <div class="searchbar-backdrop"></div>
    <div class="container">
      <div class="top-bar">
        <a href="/shopping-cart/" class="notification-bar">
          <div class="info">
            <span class="text">Good Morning</span>
            {#if $user && $user.name}
            <h2 class="title">{$user.name}</h2>
            {:else if $user && $user.email}
              <h2 class="title">{$user.email}</h2>
            {/if}     
          </div>
          <div class="media">
            <a href="" class="link panel-open pt-8" data-panel="left">
              <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <g id="surface1">
                  <path
                    fill="#5F5F5F"
                    d="M 5.671875 11.363281 C 6.324219 11.363281 6.964844 11.253906 7.574219 11.035156 C 7.773438 10.964844 7.875 10.746094 7.804688 10.546875 C 7.734375 10.347656 7.515625 10.246094 7.316406 10.316406 C 6.789062 10.503906 6.238281 10.597656 5.671875 10.597656 C 2.96875 10.597656 0.765625 8.394531 0.765625 5.6875 C 0.765625 2.980469 2.96875 0.78125 5.671875 0.78125 C 8.378906 0.78125 10.582031 2.980469 10.582031 5.6875 C 10.582031 6.238281 10.492188 6.777344 10.316406 7.289062 C 10.246094 7.492188 10.351562 7.707031 10.550781 7.777344 C 10.75 7.84375 10.96875 7.738281 11.039062 7.539062 C 11.242188 6.945312 11.347656 6.324219 11.347656 5.6875 C 11.347656 2.5625 8.800781 0.015625 5.671875 0.015625 C 2.546875 0.015625 0 2.5625 0 5.6875 C 0 8.816406 2.546875 11.363281 5.671875 11.363281 Z M 5.671875 11.363281 "
                  />
                  <path
                    fill="#5F5F5F"
                    d="M 10.574219 16.199219 C 10.375 16.269531 10.269531 16.488281 10.339844 16.6875 C 10.519531 17.210938 10.613281 17.753906 10.613281 18.3125 C 10.613281 21.019531 8.410156 23.21875 5.703125 23.21875 C 2.996094 23.21875 0.796875 21.019531 0.796875 18.3125 C 0.796875 15.605469 2.996094 13.402344 5.703125 13.402344 C 6.261719 13.402344 6.804688 13.496094 7.328125 13.675781 C 7.527344 13.746094 7.746094 13.640625 7.816406 13.441406 C 7.882812 13.242188 7.78125 13.023438 7.578125 12.957031 C 6.976562 12.746094 6.347656 12.636719 5.703125 12.636719 C 2.574219 12.636719 0.03125 15.183594 0.03125 18.3125 C 0.03125 21.4375 2.574219 23.984375 5.703125 23.984375 C 8.832031 23.984375 11.378906 21.4375 11.378906 18.3125 C 11.378906 17.667969 11.269531 17.039062 11.058594 16.433594 C 10.992188 16.234375 10.773438 16.132812 10.574219 16.199219 Z M 10.574219 16.199219 "
                  />
                  <path
                    fill="#5F5F5F"
                    d="M 18.328125 12.636719 C 17.683594 12.636719 17.054688 12.746094 16.449219 12.957031 C 16.25 13.023438 16.144531 13.242188 16.214844 13.441406 C 16.285156 13.640625 16.503906 13.746094 16.703125 13.675781 C 17.222656 13.496094 17.769531 13.402344 18.328125 13.402344 C 21.03125 13.402344 23.234375 15.605469 23.234375 18.3125 C 23.234375 21.019531 21.03125 23.21875 18.328125 23.21875 C 15.621094 23.21875 13.417969 21.019531 13.417969 18.3125 C 13.417969 17.753906 13.511719 17.210938 13.691406 16.6875 C 13.761719 16.488281 13.65625 16.269531 13.457031 16.199219 C 13.257812 16.132812 13.039062 16.234375 12.972656 16.433594 C 12.761719 17.039062 12.652344 17.667969 12.652344 18.3125 C 12.652344 21.4375 15.199219 23.984375 18.328125 23.984375 C 21.453125 23.984375 24 21.4375 24 18.3125 C 24 15.183594 21.453125 12.636719 18.328125 12.636719 Z M 18.328125 12.636719 "
                  />
                  <path
                    fill="#5F5F5F"
                    d="M 13.457031 7.800781 C 13.65625 7.730469 13.761719 7.511719 13.691406 7.3125 C 13.511719 6.789062 13.417969 6.246094 13.417969 5.6875 C 13.417969 2.980469 15.621094 0.78125 18.328125 0.78125 C 21.03125 0.78125 23.234375 2.980469 23.234375 5.6875 C 23.234375 8.394531 21.03125 10.597656 18.328125 10.597656 C 17.769531 10.597656 17.222656 10.503906 16.703125 10.324219 C 16.503906 10.253906 16.285156 10.359375 16.214844 10.558594 C 16.148438 10.757812 16.25 10.976562 16.449219 11.042969 C 17.054688 11.253906 17.683594 11.363281 18.328125 11.363281 C 21.453125 11.363281 24 8.816406 24 5.6875 C 24 2.5625 21.453125 0.015625 18.328125 0.015625 C 15.199219 0.015625 12.652344 2.5625 12.652344 5.6875 C 12.652344 6.332031 12.761719 6.960938 12.972656 7.566406 C 13.039062 7.765625 13.257812 7.867188 13.457031 7.800781 Z M 13.457031 7.800781 "
                  />
                  <path
                    fill="#5F5F5F"
                    d="M 16.945312 18.550781 C 16.972656 18.710938 17.027344 18.859375 17.101562 18.996094 L 15.398438 20.699219 C 15.25 20.847656 15.25 21.089844 15.398438 21.238281 C 15.472656 21.3125 15.570312 21.351562 15.667969 21.351562 C 15.765625 21.351562 15.863281 21.316406 15.9375 21.238281 L 17.644531 19.535156 C 17.84375 19.648438 18.078125 19.714844 18.328125 19.714844 C 19.101562 19.714844 19.730469 19.085938 19.730469 18.3125 C 19.730469 18.0625 19.664062 17.832031 19.550781 17.628906 L 21.253906 15.921875 C 21.40625 15.773438 21.40625 15.53125 21.253906 15.382812 C 21.105469 15.234375 20.863281 15.234375 20.714844 15.382812 L 19.007812 17.085938 C 18.875 17.011719 18.726562 16.957031 18.566406 16.929688 L 14.382812 12.742188 C 14.453125 12.507812 14.496094 12.257812 14.496094 12 C 14.496094 11.742188 14.453125 11.492188 14.382812 11.257812 L 18.566406 7.070312 C 18.726562 7.042969 18.875 6.988281 19.007812 6.914062 L 20.714844 8.617188 C 20.789062 8.691406 20.886719 8.730469 20.984375 8.730469 C 21.082031 8.730469 21.179688 8.691406 21.253906 8.617188 C 21.40625 8.46875 21.40625 8.226562 21.253906 8.078125 L 19.550781 6.371094 C 19.664062 6.167969 19.730469 5.9375 19.730469 5.6875 C 19.730469 4.914062 19.101562 4.285156 18.328125 4.285156 C 18.078125 4.285156 17.84375 4.351562 17.644531 4.464844 L 15.9375 2.761719 C 15.789062 2.609375 15.546875 2.609375 15.398438 2.761719 C 15.25 2.910156 15.25 3.152344 15.398438 3.300781 L 17.101562 5.003906 C 17.027344 5.140625 16.972656 5.289062 16.945312 5.449219 L 12.757812 9.632812 C 12.523438 9.558594 12.273438 9.519531 12.015625 9.519531 C 11.757812 9.519531 11.507812 9.5625 11.273438 9.632812 L 7.085938 5.449219 C 7.058594 5.289062 7.003906 5.140625 6.929688 5.003906 L 8.632812 3.300781 C 8.78125 3.152344 8.78125 2.910156 8.632812 2.761719 C 8.484375 2.609375 8.242188 2.609375 8.09375 2.761719 L 6.386719 4.464844 C 6.183594 4.351562 5.953125 4.285156 5.703125 4.285156 C 4.929688 4.285156 4.300781 4.914062 4.300781 5.6875 C 4.300781 5.9375 4.367188 6.167969 4.480469 6.371094 L 2.777344 8.078125 C 2.625 8.226562 2.625 8.46875 2.777344 8.617188 C 2.851562 8.691406 2.949219 8.730469 3.046875 8.730469 C 3.144531 8.730469 3.242188 8.691406 3.316406 8.617188 L 5.019531 6.914062 C 5.15625 6.988281 5.304688 7.042969 5.464844 7.070312 L 9.648438 11.257812 C 9.574219 11.492188 9.535156 11.742188 9.535156 12 C 9.535156 12.257812 9.574219 12.507812 9.648438 12.742188 L 5.464844 16.929688 C 5.304688 16.957031 5.15625 17.011719 5.019531 17.085938 L 3.316406 15.382812 C 3.167969 15.234375 2.925781 15.234375 2.777344 15.382812 C 2.625 15.53125 2.625 15.773438 2.777344 15.921875 L 4.480469 17.628906 C 4.367188 17.832031 4.300781 18.0625 4.300781 18.3125 C 4.300781 19.085938 4.929688 19.714844 5.703125 19.714844 C 5.953125 19.714844 6.183594 19.648438 6.386719 19.535156 L 8.09375 21.238281 C 8.167969 21.316406 8.265625 21.351562 8.363281 21.351562 C 8.460938 21.351562 8.558594 21.316406 8.632812 21.238281 C 8.78125 21.089844 8.78125 20.847656 8.632812 20.699219 L 6.929688 18.996094 C 7.003906 18.859375 7.058594 18.710938 7.085938 18.550781 L 11.273438 14.367188 C 11.507812 14.4375 11.757812 14.480469 12.015625 14.480469 C 12.273438 14.480469 12.523438 14.4375 12.757812 14.367188 Z M 18.328125 17.671875 C 18.5 17.671875 18.660156 17.746094 18.777344 17.859375 C 18.894531 17.976562 18.964844 18.136719 18.964844 18.3125 C 18.964844 18.664062 18.679688 18.949219 18.328125 18.949219 C 18.152344 18.949219 17.992188 18.878906 17.878906 18.761719 C 17.761719 18.644531 17.6875 18.484375 17.6875 18.3125 C 17.6875 17.960938 17.976562 17.671875 18.328125 17.671875 Z M 17.875 5.238281 C 17.992188 5.121094 18.152344 5.050781 18.328125 5.050781 C 18.679688 5.050781 18.964844 5.335938 18.964844 5.6875 C 18.964844 5.863281 18.894531 6.023438 18.777344 6.136719 C 18.660156 6.253906 18.5 6.328125 18.328125 6.328125 C 17.976562 6.328125 17.6875 6.039062 17.6875 5.6875 C 17.6875 5.515625 17.761719 5.355469 17.875 5.238281 Z M 17.644531 17.085938 C 17.417969 17.214844 17.230469 17.402344 17.101562 17.628906 L 13.476562 14 C 13.683594 13.851562 13.867188 13.667969 14.015625 13.460938 Z M 17.101562 6.371094 C 17.230469 6.597656 17.417969 6.785156 17.644531 6.914062 L 14.015625 10.539062 C 13.867188 10.332031 13.683594 10.148438 13.476562 10 Z M 5.703125 6.328125 C 5.527344 6.328125 5.371094 6.253906 5.253906 6.140625 C 5.136719 6.023438 5.066406 5.863281 5.066406 5.6875 C 5.066406 5.335938 5.351562 5.050781 5.703125 5.050781 C 5.878906 5.050781 6.039062 5.121094 6.152344 5.238281 C 6.269531 5.355469 6.34375 5.515625 6.34375 5.6875 C 6.34375 6.039062 6.054688 6.328125 5.703125 6.328125 Z M 6.386719 6.914062 C 6.613281 6.785156 6.800781 6.597656 6.929688 6.371094 L 10.554688 10 C 10.347656 10.148438 10.164062 10.332031 10.015625 10.539062 Z M 6.15625 18.761719 C 6.039062 18.878906 5.878906 18.949219 5.703125 18.949219 C 5.351562 18.949219 5.066406 18.664062 5.066406 18.3125 C 5.066406 18.136719 5.136719 17.976562 5.253906 17.863281 C 5.371094 17.746094 5.53125 17.671875 5.703125 17.671875 C 6.054688 17.671875 6.34375 17.960938 6.34375 18.3125 C 6.34375 18.484375 6.269531 18.644531 6.15625 18.761719 Z M 6.929688 17.628906 C 6.800781 17.402344 6.613281 17.214844 6.386719 17.085938 L 10.015625 13.460938 C 10.164062 13.667969 10.347656 13.851562 10.554688 14 Z M 12.015625 13.714844 C 11.070312 13.714844 10.300781 12.945312 10.300781 12 C 10.300781 11.054688 11.070312 10.285156 12.015625 10.285156 C 12.960938 10.285156 13.730469 11.054688 13.730469 12 C 13.730469 12.945312 12.960938 13.714844 12.015625 13.714844 Z M 12.015625 13.714844 "
                  />
                </g>
              </svg>
              <i class="fa fa-circle active pt-8"></i></a
            >
          </div>
        </a>
        <a href="#" class="link panel-open" data-panel="left">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M3 8.40009V21.0001C3 21.2653 3.10536 21.5197 3.29289 21.7072C3.48043 21.8947 3.73478 22.0001 4 22.0001H20C20.2652 22.0001 20.5196 21.8947 20.7071 21.7072C20.8946 21.5197 21 21.2653 21 21.0001V8.40009C21.0001 8.23646 20.96 8.07531 20.8833 7.93077C20.8066 7.78623 20.6956 7.66273 20.56 7.5711L12.56 2.1711C12.3946 2.05932 12.1996 1.99959 12 1.99959C11.8004 1.99959 11.6054 2.05932 11.44 2.1711L3.44 7.5711C3.30443 7.66273 3.19342 7.78623 3.11671 7.93077C3.03999 8.07531 2.99992 8.23646 3 8.40009ZM14 20.0001H10V14.0001H14V20.0001ZM5 8.9321L12 4.2071L19 8.9321V20.0001H16V13.0001C16 12.7349 15.8946 12.4805 15.7071 12.293C15.5196 12.1055 15.2652 12.0001 15 12.0001H9C8.73478 12.0001 8.48043 12.1055 8.29289 12.293C8.10536 12.4805 8 12.7349 8 13.0001V20.0001H5V8.9321Z"
              fill="#0F0F0F"
            />
          </svg>
        </a>
      </div>

      <!-- Search -->
      <form
        data-search-container=".search-list"
        data-search-in=".item-title"
        class="searchbar searchbar-init search-box list-search-bx"
      >
        <div class="searchbar-inner">
          <div class="searchbar-input-wrap">
            <input type="search" placeholder="Search beverages or foods" />
            <i class="searchbar-icon" style="width: 24px; height: 24px;">
              <svg
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M9.65925 19.3102C11.8044 19.3103 13.8882 18.5946 15.5806 17.2764L21.9653 23.6612C22.4423 24.1218 23.2023 24.1086 23.663 23.6316C24.1123 23.1664 24.1123 22.4288 23.663 21.9636L17.2782 15.5788C20.5491 11.3682 19.7874 5.30335 15.5769 2.03244C11.3663 -1.23846 5.30149 -0.476784 2.03058 3.73376C-1.24033 7.9443 -0.478646 14.0092 3.73189 17.2801C5.42702 18.597 7.51269 19.3113 9.65925 19.3102ZM4.52915 4.52732C7.36245 1.69396 11.9561 1.69391 14.7895 4.52721C17.6229 7.36052 17.6229 11.9542 14.7896 14.7876C11.9563 17.6209 7.36261 17.621 4.52925 14.7877C4.5292 14.7876 4.5292 14.7876 4.52915 14.7876C1.69584 11.9749 1.67915 7.39796 4.49181 4.56465C4.50424 4.55217 4.51667 4.53974 4.52915 4.52732Z"
                  fill="#C9C9C9"
                />
              </svg>
            </i>
          </div>
        </div>
      </form>
      <div class="list simple-list searchbar-not-found">
        <ul>
          <li>Nothing found</li>
        </ul>
      </div>

      <!-- Products -->
      <swiper-container
        space-between={20}
        slides-per-view={"auto"}
        centered-slides={false}
        class="swiper swiper-init main-swiper mb-40 overlay-swiper1 demo-swiper-multiple demo-swiper-multiple-auto">
        <div class="swiper-slide">
          <div class="card-overlay style-1 bg-primary"><a href="/item-details-pizza/">
            <div class="dz-media">
              <img src="./assets/img/products/featured/pic1.png" alt=""/>
            </div>
            <a href="/shopping-cart/">
            <div class="dz-info">
              <h5 class="item-title title">Pizza Hut L Queso</h5>
              <div class="dz-meta">
                <ul>
                  <li class="price"><sup>$</sup>500 <del>$899</del></li>
                </ul>
              </div>
            </div></a>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="card-overlay style-1 bg-primary">
            <a href="/item-details-tea/">
              <div class="dz-media">
                <img src="./assets/img/products/featured/pic2.png" alt=""/>
              </div>
              <div class="dz-info">
                <h5 class="item-title title">Indonesian Tea</h5>
                <div class="dz-meta">
                  <ul>
                    <li class="price">$150 <del>$304</del></li>
                  </ul>
                </div>
              </div>
            </a>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="card-overlay style-1 bg-primary"><a href="/item-details-kfc/">
            <div class="dz-media">
              <img src="./assets/img/products/featured/pic3.png" alt=""/>
            </div>
            <div class="dz-info">
              <h5 class="item-title title">KFC Combo Familia</h5>
              <div class="dz-meta">
                <ul>
                  <li class="price"><sup>$</sup>750 <del>$1220</del></li>
                </ul>
              </div>
            </div></a>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="card-overlay style-1 bg-primary"><a href="/item-details-pills/">
            <div class="dz-media">
              <img src="./assets/img/products/featured/pic4.png" alt=""/>
            </div>
            <div class="dz-info">
              <h5 class="item-title title">Tylenol Ultra</h5>
              <div class="dz-meta">
                <ul>
                  <li class="price">$25 <del>$54</del></li>
                </ul>
              </div>
            </div></a>
          </div>
        </div>
        <div class="swiper-slide">
          <div class="card-overlay style-1 bg-primary"><a href="/item-details-bon/">
            <div class="dz-media">
              <img src="./assets/img/products/featured/pic5.png" alt=""/>
            </div>
            <div class="dz-info">
              <h5 class="item-title title">BON XGrande</h5>
              <div class="dz-meta">
                <ul>
                  <li class="price"><sup>$</sup>275 <del>$499</del></li>
                </ul>
              </div>
            </div></a>
          </div>
        </div>
        <!-- Display the favourite food items using loop -->
        {#each foodItems as foodItem}
          {#if foodItem.Food_Tag == FOOD_TAG_FEATURED}
            <swiper-slide style="width:auto" class="pl-4">
              <div class="card-overlay style-1 bg-primary">
                <div class="dz-media">
                  <a href="/item-detail/{foodItem.$id}">
                    <img src={foodItem.Food_Image} alt="No Img" />
                  </a>
                </div>
                <div class="dz-info">
                  <h5 class="item-title title">
                    <a href="/item-detail/{foodItem.$id}">{foodItem.Food_Title}</a>
                  </h5>
                  <div class="dz-meta">
                    <ul>
                      <li class="price">
                        ${foodItem.Food_Price}<del>$699</del>
                      </li>
                    </ul>
                  </div>
                </div>
              </div>
            </swiper-slide>
          {/if}
        {/each}
      </swiper-container>

      <!-- Categories -->
      <div class="title-bar">
        <h4 class="dz-title">Categories</h4>
      </div>
      <swiper-container
        space-between={20}
        slides-per-view={"auto"}
        centered-slides={false}
        class="swiper swiper-init mb-40 main-swiper categories-swiper demo-swiper-multiple demo-swiper-multiple-auto"
      >
        <!-- <div class="swiper-wrapper"> -->
        <swiper-slide style="width:auto" class=" pl-4">
          <div class="categories-bx">
            <div class="icon-bx">
              <a href="/products/">
                <svg
                  enable-background="new 0 0 48 48"
                  height="24"
                  viewBox="0 0 48 48"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m30.1 47.5h-21.8c-.9 0-1.7-.7-1.9-1.6l-5.9-30.5c-.1-.6 0-1.2.4-1.6.4-.5.9-.7 1.5-.7h33.5c.6 0 1.1.3 1.5.7s.5 1 .4 1.6l-5.8 30.5c-.2.9-1 1.6-1.9 1.6zm-20.2-3.9h18.6l5.1-26.6h-28.8z"
                    fill="#fff"
                  />
                  <path
                    d="m31.3 42.3c-.5 0-1.1-.2-1.5-.7-.7-.8-.6-2.1.2-2.8 3.9-3.4 6.1-5.5 9-8.2 1-.9 2-1.9 3.2-3 1.8-1.7 1.4-3.4.9-4.2-.9-1.7-3.3-3.1-6.5-2.4-1.1.2-2.1-.4-2.3-1.5s.4-2.1 1.5-2.3c4.5-1 8.9.9 10.8 4.5 1.6 3 .9 6.4-1.8 8.9-1.2 1.1-2.2 2.1-3.2 3-2.8 2.6-5.2 4.9-9.1 8.3-.3.2-.7.4-1.2.4z"
                    fill="#fff"
                  />
                  <path
                    d="m9.3 10.1c-1.1 0-2-.9-2-2v-5.6c0-1.1.9-2 2-2s2 .9 2 2v5.7c-.1 1-.9 1.9-2 1.9z"
                    fill="#fff"
                  />
                  <path
                    d="m18.1 10.1c-1.1 0-2-.9-2-2v-5.6c0-1.1.9-2 2-2s2 .9 2 2v5.7c-.1 1-.9 1.9-2 1.9z"
                    fill="#fff"
                  />
                  <path
                    d="m26.9 10.1c-1.1 0-2-.9-2-2v-5.6c0-1.1.9-2 2-2s2 .9 2 2v5.7c-.1 1-.9 1.9-2 1.9z"
                    fill="#fff"
                  />
                </svg>
              </a>
            </div>
            <div class="dz-content">
              <h5 class="title"><a href="/products/">Beverages</a></h5>
              <span class="menus text-primary">67 Menus</span>
            </div>
          </div>
        </swiper-slide>
        <swiper-slide style="width:auto">
          <div class="categories-bx">
            <div class="icon-bx">
              <a href="/products/">
                <svg
                  enable-background="new 0 0 48 48"
                  height="24"
                  viewBox="0 0 48 48"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m43 47.5h-38c-2.5 0-4.5-1.9-4.5-4.1v-9.8c0-1.1.9-2 2-2h43.1c1.1 0 2 .9 2 2v9.8c-.1 2.2-2.1 4.1-4.6 4.1zm-38.6-12v7.8s.2.2.6.2h38c.4 0 .6-.2.6-.3v-7.8h-39.2z"
                    fill="#fff"
                  />
                  <path
                    d="m45.5 23.8h-43c-1.1 0-2-.9-2-2v-3.8c0-9.6 7.9-17.5 17.5-17.5h12c9.7 0 17.5 7.9 17.5 17.5v3.8c0 1.1-.9 2-2 2zm-41.1-4h39.2v-1.8c0-7.5-6.1-13.6-13.6-13.6h-12c-7.5 0-13.6 6.1-13.6 13.6z"
                    fill="#fff"
                  />
                  <path
                    d="m45.5 30h-43c-1.1 0-2-.9-2-2s.9-2 2-2h43.1c1.1 0 2 .9 2 2s-1 2-2.1 2z"
                    fill="#fff"
                  />
                </svg>
              </a>
            </div>
            <div class="dz-content">
              <h5 class="title"><a href="/products/">Foods</a></h5>
              <span class="menus text-primary">23 Menus</span>
            </div>
          </div>
        </swiper-slide>
        <swiper-slide style="width:auto">
          <div class="categories-bx">
            <div class="icon-bx">
              <a href="/products/">
                <svg
                  enable-background="new 0 0 512 512"
                  height="24"
                  viewBox="0 0 512 512"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m512 383.541-.772-12.458c-2.975-48.023-14.428-94.406-34.039-137.861-19.652-43.545-46.972-82.98-81.201-117.209-34.229-34.23-73.664-61.549-117.209-81.202-43.455-19.611-89.839-31.064-137.862-34.039l-12.458-.772-128.459 512zm-360.535-351.809c84.177 8.108 163.058 45.294 223.284 105.52 60.226 60.227 97.411 139.107 105.519 223.284l-61.408 15.407c1.003-3.449 2.134-6.986 3.286-10.589 5.485-17.149 11.156-34.883 6.3-52.992-5.043-18.808-19.777-32.091-32.776-43.81-8.372-7.548-16.28-14.677-20.571-22.083-4.421-7.628-6.717-18.191-9.147-29.375-3.683-16.945-7.857-36.151-21.377-49.671-13.521-13.52-32.726-17.694-49.671-21.376-11.183-2.43-21.746-4.726-29.375-9.147-7.406-4.291-14.535-12.199-22.083-20.571-11.719-12.999-25.002-27.732-43.809-32.775-4.833-1.296-9.639-1.842-14.415-1.842-13.118 0-26.004 4.121-38.577 8.142-3.602 1.152-7.14 2.284-10.589 3.286zm-59.607 237.575c14.851 7.405 23.258 24.455 19.223 41.158-2.284 9.458-8.115 17.46-16.418 22.532-6.142 3.752-13.092 5.563-20.122 5.33zm-24.718 98.52c2.886.379 5.776.575 8.656.575 12.077 0 23.958-3.316 34.526-9.772 15.15-9.255 25.788-23.855 29.956-41.111 4.12-17.058 1.417-34.737-7.611-49.781-7.77-12.948-19.471-22.674-33.405-27.937l28.599-113.988c9.575-1.485 18.866-4.448 27.936-7.349 13.616-4.355 26.48-8.468 36.063-5.897 10.227 2.742 19.482 13.008 29.28 23.876 8.77 9.728 17.839 19.787 29.331 26.447 11.705 6.784 25.101 9.695 38.056 12.51 14.158 3.076 27.53 5.983 34.811 13.264 7.281 7.282 10.188 20.654 13.264 34.811 2.815 12.955 5.727 26.351 12.51 38.056 6.66 11.492 16.719 20.561 26.447 29.331 10.868 9.798 21.134 19.053 23.876 29.281 2.57 9.586-1.542 22.446-5.897 36.062-2.901 9.07-5.864 18.361-7.349 27.936l-107.016 26.85c-6.4-13.287-16.567-24.4-29.553-32.07-17.352-10.25-37.657-13.128-57.175-8.107-19.364 4.983-35.679 17.176-45.939 34.335-7.647 12.788-11.271 27.234-10.65 41.8l-94.516 23.714zm181.832 50.74-82.88 20.794c1.891-18.392 15.001-34.613 33.836-39.46 11.749-3.024 23.969-1.29 34.414 4.879 5.942 3.51 10.9 8.219 14.63 13.787z"
                    fill="#fff"
                  />
                  <path
                    d="m239.746 345.773c40.538 0 73.519-32.981 73.519-73.519s-32.981-73.519-73.519-73.519-73.519 32.981-73.519 73.519 32.981 73.519 73.519 73.519zm0-117.002c23.977 0 43.483 19.506 43.483 43.483s-19.506 43.483-43.483 43.483-43.483-19.506-43.483-43.483 19.507-43.483 43.483-43.483z"
                    fill="#fff"
                  />
                  <path
                    d="m148.74 164.819h28.701v30.036h-28.701z"
                    fill="#fff"
                  />
                  <path
                    d="m312.176 339.529h28.701v30.036h-28.701z"
                    fill="#fff"
                  />
                  <path
                    d="m86.762 395.869h28.701v30.036h-28.701z"
                    fill="#fff"
                  />
                </svg>
              </a>
            </div>
            <div class="dz-content">
              <h5 class="title"><a href="/products/">Pizza</a></h5>
              <span class="menus text-primary">28 Menus</span>
            </div>
          </div>
        </swiper-slide>
        <swiper-slide style="width:auto">
          <div class="categories-bx">
            <div class="icon-bx">
              <a href="/products/">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  x="0"
                  y="0"
                  viewBox="0 0 510 510"
                  style="enable-background:new 0 0 512 512"
                  xml:space="preserve"
                >
                  <path
                    d="m510 30v-30h-116.079l-27.692 90h-217.427l16.154 210h-164.956v45c0 90.981 74.019 165 165 165h283.891l32.308-420h-83.581l18.462-60zm-153.002 90-23.077 75h-146.954l-5.769-75zm-167.723 105h251.449l-15 195h-113.776c11.537-22.515 18.052-48.011 18.052-75v-45h-134.956zm-93.278 149.373c-3.856-9.025-5.997-18.953-5.997-29.373v-15h42.975zm54.003-17.942v62.06c-13.795-2.811-26.215-9.432-36.136-18.696zm30 0 36.136 43.363c-9.921 9.265-22.34 15.886-36.136 18.696zm60-11.431c0 10.42-2.141 20.348-5.997 29.373l-36.978-44.373h42.975zm-210 0v-15h30v15c0 57.897 47.103 105 105 105s105-47.103 105-105v-15h30v15c0 74.439-60.561 135-135 135s-135-60.561-135-135zm229.758 135c12.102-8.52 23.015-18.617 32.429-30h131.23l-2.308 30zm189.044-360-5.77 75h-77.722l23.077-75z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                  <path
                    d="m375 270h30v30h-30z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                  <path
                    d="m360 345h30v30h-30z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                </svg>
              </a>
            </div>
            <div class="dz-content">
              <h5 class="title"><a href="/products/">Drink</a></h5>
              <span class="menus text-primary">12 Menus</span>
            </div>
          </div>
        </swiper-slide>
        <swiper-slide style="width:auto">
          <div class="categories-bx">
            <div class="icon-bx">
              <a href="/products/">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  x="0"
                  y="0"
                  viewBox="0 0 512 512"
                  style="enable-background:new 0 0 512 512"
                  xml:space="preserve"
                >
                  <path
                    d="m433.609 152.564c1.742.696 2.484 2.064 2.77 2.818.286.753.642 2.268-.198 3.947l-16.12 35.271 27.396 12.512 15.796-34.587c4.245-8.679 4.712-18.809 1.276-27.847-3.472-9.132-10.666-16.45-19.736-20.078-2.257-.902-3.494-3.312-2.974-5.638l8.689-19.358-27.475-12.334-9.412 20.969-.239.717c-5.857 17.572 3.027 36.728 20.227 43.608z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                  <path
                    d="m323.205 65.725c1.743.697 2.485 2.065 2.771 2.818s.642 2.268-.198 3.947l-16.121 35.272 27.396 12.512 15.796-34.585c4.245-8.679 4.713-18.81 1.277-27.849-3.473-9.133-10.666-16.45-19.736-20.077-2.258-.903-3.496-3.313-2.974-5.638l8.688-19.359-27.476-12.335-9.411 20.968-.239.716c-5.857 17.573 3.026 36.728 20.227 43.61z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                  <path
                    d="m177.609 65.725c1.742.696 2.484 2.064 2.77 2.818.286.753.642 2.268-.198 3.947l-16.12 35.271 27.396 12.512 15.796-34.587c4.245-8.679 4.712-18.809 1.276-27.847-3.472-9.132-10.666-16.45-19.736-20.078-2.257-.902-3.494-3.312-2.974-5.638l8.689-19.358-27.476-12.334-9.411 20.969-.239.717c-5.857 17.572 3.027 36.728 20.227 43.608z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                  <path
                    d="m67.205 147.042c1.743.697 2.485 2.065 2.771 2.818s.642 2.268-.198 3.947l-16.121 35.271 27.396 12.512 15.796-34.585c4.245-8.679 4.713-18.81 1.277-27.849-3.473-9.133-10.666-16.45-19.736-20.077-2.258-.903-3.496-3.313-2.974-5.638l8.688-19.359-27.476-12.334-9.411 20.968-.239.716c-5.857 17.574 3.026 36.729 20.227 43.61z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                  <path
                    d="m481.376 421.216c-3.575-54.667-26.56-105.569-65.654-144.664-31.809-31.809-71.436-52.956-114.546-61.66v-19.558c0-24.91-20.266-45.176-45.176-45.176-24.911 0-45.176 20.266-45.176 45.176v19.558c-43.109 8.704-82.737 29.851-114.546 61.66-39.094 39.094-62.08 89.998-65.653 144.664h-30.625v90.353h512v-90.353zm-240.435-210.319v-15.563c0-8.303 6.754-15.059 15.059-15.059 8.303 0 15.059 6.755 15.059 15.059v15.563c-9.609-.687-19.674-.647-30.118 0zm41.502 31.406c90.595 12.288 161.706 86.776 168.748 178.914h-390.382c7.043-92.138 78.153-166.625 168.748-178.914 17.615-2.088 35.243-2.17 52.886 0zm199.439 239.149h-451.764v-30.118h451.765v30.118z"
                    fill="#ffffff"
                    data-original="#000000"
                  />
                </svg>
              </a>
            </div>
            <div class="dz-content">
              <h5 class="title"><a href="/products/">Lunch</a></h5>
              <span class="menus text-primary">67 Menus</span>
            </div>
          </div>
        </swiper-slide>
        <swiper-slide style="width:auto">
          <div class="categories-bx">
            <div class="icon-bx">
              <a href="/products/">
                <svg
                  enable-background="new 0 0 48 48"
                  height="24"
                  viewBox="0 0 48 48"
                  width="24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m43 47.5h-38c-2.5 0-4.5-1.9-4.5-4.1v-9.8c0-1.1.9-2 2-2h43.1c1.1 0 2 .9 2 2v9.8c-.1 2.2-2.1 4.1-4.6 4.1zm-38.6-12v7.8s.2.2.6.2h38c.4 0 .6-.2.6-.3v-7.8h-39.2z"
                    fill="#fff"
                  />
                  <path
                    d="m45.5 23.8h-43c-1.1 0-2-.9-2-2v-3.8c0-9.6 7.9-17.5 17.5-17.5h12c9.7 0 17.5 7.9 17.5 17.5v3.8c0 1.1-.9 2-2 2zm-41.1-4h39.2v-1.8c0-7.5-6.1-13.6-13.6-13.6h-12c-7.5 0-13.6 6.1-13.6 13.6z"
                    fill="#fff"
                  />
                  <path
                    d="m45.5 30h-43c-1.1 0-2-.9-2-2s.9-2 2-2h43.1c1.1 0 2 .9 2 2s-1 2-2.1 2z"
                    fill="#fff"
                  />
                </svg>
              </a>
            </div>
            <div class="dz-content">
              <h5 class="title"><a href="/products/">Burger</a></h5>
              <span class="menus text-primary">22 Menus</span>
            </div>
          </div>
        </swiper-slide>
        <!-- </div> -->
      </swiper-container>

      <!-- Featured Beverages -->
      <div class="title-bar">
        <h4 class="dz-title">Mis Promociones</h4>
        <a href="/products/" class="button text-capitalize">More</a>
      </div>
      <div class="list search-list mt-0 mb-20 searchbar-found item-list">
        <ul class="grid grid-cols-1 medium-grid-cols-2 grid-gap">
          <li class="col-100 medium-50">
						<div class="item-bx item-list">
							<div class="dz-media">
								<a href="/item-details-pizza/"><img src="./assets/img/products/product1a.jpg" alt=""/></a>
								<div class="rating"><i class="fa fa-star"></i> 3.8</div>
							</div>
							<div class="dz-info">
								<div class="dz-head">
									<h6 class="item-title"><a href="/item-details-pizza/">Burger King Villa Mella</a></h6>
								</div>
								<div class="dz-meta">
									<ul>
										<li class="price">-5%</li>
										<li class="pts text-primary">50 Pts</li>
									</ul>
								</div>
							</div>
						</div>
					</li>
					<li class="col-100 medium-50">
						<div class="item-bx item-list">
							<div class="dz-media">
								<a href="/item-details-pizza/"><img src="./assets/img/products/product2a.jpg" alt=""/></a>
								<div class="rating"><i class="fa fa-star"></i> 3.8</div>
							</div>
							<div class="dz-info">
								<div class="dz-head">
									<h6 class="item-title"><a href="/item-details-pizza/">Farmacia Popular Zona Colonial</a></h6>
								</div>
								<div class="dz-meta">
									<ul>
										<li class="price">-20%</li>
										<li class="pts text-primary">50 Pts</li>
									</ul>
								</div>
							</div>
						</div>
					</li>
          <!-- Display the popular food items using loop -->
          {#each foodItems as foodItem}
            {#if foodItem.Food_Tag == FOOD_TAG_POPULAR}
              <li>
                <div class="item-bx item-list">
                  <div class="dz-media">
                    <a href="/item-detail/{foodItem.$id}">
                      <img src={foodItem.Food_Image} alt="" />
                    </a>
                    <div class="rating"><i class="fa fa-star"></i> 3.8</div>
                  </div>
                  <div class="dz-info">
                    <div class="dz-head">
                      <h6 class="item-title">
                        <a href="/item-detail/{foodItem.$id}">{foodItem.Food_Title}</a>
                      </h6>
                    </div>
                    <div class="dz-meta">
                      <ul>
                        <li class="price">{foodItem.Food_Price}</li>
                        <li class="pts text-primary">
                          {foodItem.Food_Description}
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
              </li>
            {/if}
          {/each}
        </ul>
      </div>
    </div>
  </div>
</div>
