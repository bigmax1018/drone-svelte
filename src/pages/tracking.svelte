<script>
  import { orders, orderStatusStore } from "./../js/fakeOrder.js";
  import { Link } from "framework7-svelte";
  import {
    foodStore,
    businessLocationStore,
    dropoffLocationStore,
  } from "../js/store.js";
  import { onDestroy, onMount } from "svelte";

  let foodValue;
  let businessLocation;
  let dropoffLocation;
  let orderStatus;
  let orderSubmitted = false;
  let order;

  let unsubscribeFoodStore = foodStore.subscribe((value) => {
    foodValue = value;
  });

  let unsubscribeBusinessLocationStore = businessLocationStore.subscribe(
    (value) => {
      businessLocation = value;
    },
  );

  let unsubscribeDropoffLocationStore = dropoffLocationStore.subscribe(
    (value) => {
      dropoffLocation = value;
    },
  );

  let unsubscribeOrderStatusStore = orderStatusStore.subscribe((value) => {
    orderStatus = value;
  });

  $: if (businessLocation && dropoffLocation && foodValue && !orderSubmitted) {
    console.log("Food Variable:", foodValue);
    console.log("businessLocation:", businessLocation);
    console.log("dropoffLocation:", dropoffLocation);
    order = orders.createFakeOrder(
      foodValue,
      businessLocation,
      dropoffLocation,
    );
    orders.startOrderStatusUpdates();
    orderSubmitted = true;
  }

  onMount(async () => {
    console.clear();
  });

  onDestroy(() => {
    unsubscribeBusinessLocationStore();
    unsubscribeDropoffLocationStore();
    unsubscribeOrderStatusStore();
    unsubscribeFoodStore();
  });
</script>

<div class="page location-tracking">
  <div class="navbar navbar-style-1">
    <div class="navbar-inner">
      <Link back>
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M0.439312 13.0606L5.75391 18.3752C6.04683 18.6682 6.43069 18.8146 6.81459 18.8146C7.1985 18.8146 7.58236 18.6682 7.87528 18.3752C8.46103 17.7894 8.46103 16.8397 7.87528 16.2539L5.12133 13.5H22.5C23.3284 13.5 24 12.8284 24 12C24 11.1716 23.3284 10.5 22.5 10.5H5.12133L7.87528 7.7461C8.46103 7.1603 8.46103 6.21057 7.87528 5.62477C7.28944 5.03898 6.33975 5.03898 5.75391 5.62477L0.439312 10.9394C-0.146437 11.5251 -0.146437 12.4749 0.439312 13.0606Z"
            fill="black"
          />
        </svg>
      </Link>
      <div class="flex">Tracking Orders</div>
      <div class="right">
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
              fill="#04764E"
            />
          </svg>
        </a>
      </div>
    </div>
  </div>

  <div class="page-content pt-0">
    <!-- Map -->
    <iframe
      src="https://www.google.com/maps/embed/v1/place?q=Santo+Domingo,+Dominican+Republic&key=AIzaSyBFw0Qbyq9zTFTd-tUY6dZWTgaQzuU17R8"
      allowfullscreen=""
      loading="lazy"
      title="map"
    ></iframe>

    <div class="bottom-wraper">
      <img src="../assets/dronegif.gif" alt="drone" class="w-[60%] mb-48" />
      <div class="container track-content">
        <div class="map-dir">
          <p class="ma-0">Estimated Time</p>
          <h4 class="ma-0">5-10 min</h4>
        </div>
      </div>
      <div class="delivery-man container bg-primary">
        <div class="inner-content">
          <div class="dz-media">
            <img src="../assets/img/avatar/avatar1.jpg" alt="" />
          </div>
          <div class="dz-content">
            <h5 class="drone-id text-white">Drone #313</h5>
            <p class="text-white">"Sally"</p>
          </div>
          <div class="icon-area">
            <a href="#" class="icon-cell"
              ><center>
                <svg
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M9.48457 7.02438C10.0416 5.74586 9.75992 4.25673 8.77447 3.26995L6.49912 0.995688C5.74323 0.23078 4.66561 -0.12394 3.60315 0.0423647C2.52869 0.200165 1.59539 0.86444 1.09435 1.82797C-0.00580866 3.89842 -0.28881 6.30639 0.301383 8.57545C1.75537 14.3347 9.6711 22.2559 15.4293 23.699C16.2065 23.8961 17.0052 23.9965 17.8071 23.9979C19.3309 23.9969 20.8313 23.6224 22.1767 22.9071C23.8369 22.0413 24.4808 19.9937 23.615 18.3336C23.4561 18.0288 23.2518 17.75 23.009 17.5067L20.7347 15.2313C19.7476 14.2457 18.258 13.9641 16.9792 14.5212C16.3824 14.7784 15.8381 15.1434 15.3736 15.5978C14.8173 16.1574 12.7656 15.3469 10.7116 13.2941C8.65768 11.2413 7.84945 9.18955 8.40246 8.63106C8.85876 8.16645 9.22557 7.62185 9.48457 7.02438ZM9.16609 14.8387C11.6672 17.3376 15.0312 19.0228 16.9106 17.1424C17.1874 16.8774 17.51 16.6648 17.8628 16.5152C18.3159 16.3221 18.8409 16.4254 19.187 16.7759L21.4613 19.0501C21.7323 19.3181 21.858 19.7 21.7994 20.0766C21.7424 20.4636 21.5024 20.7993 21.1547 20.9786C19.562 21.8345 17.7031 22.0513 15.9561 21.5851C11.0084 20.3372 3.66858 12.9975 2.42078 8.04318C1.95503 6.29467 2.17226 4.43442 3.02836 2.84021C3.20898 2.49446 3.54447 2.25634 3.93042 2.19993C3.99066 2.19034 4.05156 2.18557 4.1126 2.18576C4.4298 2.18566 4.73383 2.31252 4.95683 2.53807L7.23109 4.81234C7.58104 5.15822 7.68442 5.68265 7.4918 6.13545C7.34283 6.49055 7.13027 6.8155 6.86461 7.09425C4.98404 8.96925 6.66712 12.3299 9.16609 14.8387Z"
                    fill="white"
                  />
                </svg></center
              >
            </a>
            <a href="/messages/" class="icon-cell -mr-4"
              ><center>
                <svg
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M11.9999 22C13.7263 22.0016 15.4233 21.5535 16.9239 20.7L20.6839 21.953C20.786 21.9857 20.8927 22.0016 20.9999 22C21.1583 21.9999 21.3144 21.9621 21.4553 21.8899C21.5962 21.8177 21.718 21.713 21.8106 21.5845C21.9031 21.456 21.9639 21.3074 21.9877 21.1508C22.0116 20.9942 21.998 20.8343 21.9479 20.684L20.6999 16.924C21.7809 15.012 22.2019 12.7972 21.898 10.6219C21.5941 8.44663 20.5821 6.43204 19.0184 4.88955C17.4548 3.34706 15.4266 2.36257 13.2474 2.08826C11.0682 1.81395 8.85934 2.26509 6.96222 3.37196C5.06511 4.47883 3.58541 6.17978 2.75187 8.21187C1.91833 10.244 1.77736 12.494 2.35075 14.6143C2.92414 16.7345 4.17996 18.6069 5.92409 19.9419C7.66822 21.2768 9.80352 22.0002 11.9999 22ZM11.9999 4C13.4315 3.99799 14.8374 4.38072 16.0704 5.10814C17.3035 5.83557 18.3184 6.88096 19.009 8.13498C19.6997 9.389 20.0407 10.8056 19.9963 12.2365C19.952 13.6674 19.5239 15.0602 18.7569 16.269C18.6782 16.3946 18.6285 16.5361 18.6112 16.6832C18.5939 16.8304 18.6095 16.9796 18.6569 17.12L19.4229 19.42L17.1229 18.654C16.9826 18.6059 16.8333 18.59 16.6861 18.6073C16.5388 18.6246 16.3973 18.6747 16.2719 18.754C15.2234 19.4196 14.0343 19.832 12.7988 19.9584C11.5633 20.0849 10.3153 19.9219 9.15371 19.4825C7.9921 19.043 6.94875 18.3392 6.10629 17.4266C5.26384 16.5141 4.64541 15.4179 4.29999 14.225C3.95458 13.032 3.89165 11.775 4.1162 10.5535C4.34076 9.33206 4.84662 8.17963 5.59373 7.18752C6.34084 6.19541 7.30867 5.39087 8.42059 4.83761C9.53251 4.28436 10.758 3.99758 11.9999 4Z"
                    fill="white"
                  />
                  <path
                    d="M8 10.5H16C16.2652 10.5 16.5196 10.3946 16.7071 10.2071C16.8946 10.0196 17 9.76521 17 9.5C17 9.23478 16.8946 8.98043 16.7071 8.79289C16.5196 8.60536 16.2652 8.5 16 8.5H8C7.73478 8.5 7.48043 8.60536 7.29289 8.79289C7.10536 8.98043 7 9.23478 7 9.5C7 9.76521 7.10536 10.0196 7.29289 10.2071C7.48043 10.3946 7.73478 10.5 8 10.5Z"
                    fill="white"
                  />
                  <path
                    d="M8 15.5H12C12.2652 15.5 12.5196 15.3947 12.7071 15.2071C12.8946 15.0196 13 14.7652 13 14.5C13 14.2348 12.8946 13.9804 12.7071 13.7929C12.5196 13.6054 12.2652 13.5 12 13.5H8C7.73478 13.5 7.48043 13.6054 7.29289 13.7929C7.10536 13.9804 7 14.2348 7 14.5C7 14.7652 7.10536 15.0196 7.29289 15.2071C7.48043 15.3947 7.73478 15.5 8 15.5Z"
                    fill="white"
                  />
                </svg></center>
            </a>
<a href="javascript:void(0)" class="ml-8 mt-2" on:click={(e) =>
    (e.currentTarget.nextElementSibling.style.display =
      e.currentTarget.nextElementSibling.style.display === "none" ? "block" : "none")}>
    <svg
    width="24"
    height="28"
    viewBox="0 0 12 12"
      fill="none"
      xmlns="http://www.w3.org/2000/svg">
      <path
        d="M8.625 2.5C8.625 3.12132 8.12132 3.625 7.5 3.625C6.87868 3.625 6.375 3.12132 6.375 2.5C6.375 1.87868 6.87868 1.375 7.5 1.375C8.12132 1.375 8.625 1.87868 8.625 2.5ZM8.625 7.5C8.625 8.12132 8.12132 8.625 7.5 8.625C6.87868 8.625 6.375 8.12132 6.375 7.5C6.375 6.87868 6.87868 6.375 7.5 6.375C8.12132 6.375 8.625 6.87868 8.625 7.5ZM7.5 13.625C8.12132 13.625 8.625 13.1213 8.625 12.5C8.625 11.8787 8.12132 11.375 7.5 11.375C6.87868 11.375 6.375 11.8787 6.375 12.5C6.375 13.1213 6.87868 13.625 7.5 13.625Z"
        fill="white"/>
    </svg>
</a>

<div class="options-box" style="display: none;">
    <button
      type="button"
      on:click={() => {
        orders.cancelOrder();
      }}
      class="btn-primary mt-1 text-black bg-red-500 rounded-2xl font-extrabold mx-2"
      >Cancel Order</button
    >
</div>
          </div>
        </div>
      </div>

      <div class="fixed-content container">
        <ul class="track-list">
          <li>
            <div class="icon-bx location">
              <center>
                <svg
                  width="24"
                  height="24"
                  viewBox="0 0 24 24"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M22.029,5.994,20.97,1.758A1,1,0,0,0,20,1H4a1,1,0,0,0-.97.758L1.971,5.994A4,4,0,0,0,2.7,9.45a4.033,4.033,0,0,0,.3.3V22a1,1,0,0,0,1,1H20a1,1,0,0,0,1-1V9.753a4.033,4.033,0,0,0,.3-.3A4,4,0,0,0,22.029,5.994ZM10,6l.251-3h3.492L14,6l.066.8A2.027,2.027,0,0,1,12.042,9h-.084A2.027,2.027,0,0,1,9.937,6.8ZM4.28,8.22a2.016,2.016,0,0,1-.369-1.741L4.781,3H8.247L7.9,7.14A2.041,2.041,0,0,1,5.879,9,2.015,2.015,0,0,1,4.28,8.22ZM13,21H11V16h2Zm6,0H15V15a1,1,0,0,0-1-1H10a1,1,0,0,0-1,1v6H5V10.9A3.953,3.953,0,0,0,8.911,9.589c.03.035.051.076.083.11A4.04,4.04,0,0,0,11.958,11h.084a4.04,4.04,0,0,0,2.964-1.3c.032-.034.053-.075.083-.11A3.953,3.953,0,0,0,19,10.9Zm.72-12.78a2.015,2.015,0,0,1-1.6.78A2.041,2.041,0,0,1,16.1,7.14L15.753,3h3.466l.87,3.479A2.016,2.016,0,0,1,19.72,8.22Z"
                  />
                </svg></center
              >
            </div>
            {#if orderStatus}
              <div class="info">
                <h5 class="order-status">Order Status:</h5>
                <p>{orderStatus}</p>
              </div>
            {:else}
              <div class="info">
                <h5 class="order-status">Awaiting Order...</h5>
              </div>
            {/if}
          </li>
          <li>
            <div class="icon-bx">
              <center>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 24 24"
                  width="24"
                  height="24"
                >
                  <path
                    d="M11.9993 5.48404C9.59314 5.48404 7.64258 7.4346 7.64258 9.84075C7.64258 12.2469 9.59314 14.1975 11.9993 14.1975C14.4054 14.1975 16.356 12.2469 16.356 9.84075C16.356 7.4346 14.4054 5.48404 11.9993 5.48404ZM11.9993 12.0191C10.7962 12.0191 9.82096 11.0438 9.82096 9.84075C9.82096 8.6377 10.7962 7.66242 11.9993 7.66242C13.2023 7.66242 14.1776 8.6377 14.1776 9.84075C14.1776 11.0438 13.2023 12.0191 11.9993 12.0191Z"
                    fill="#4A3749"
                  />
                  <path
                    d="M21.793 9.81896C21.8074 4.41054 17.4348 0.0144869 12.0264 5.09008e-05C6.61797 -0.0143851 2.22191 4.35827 2.20748 9.76664C2.16044 15.938 5.85106 21.5248 11.546 23.903C11.6884 23.9674 11.8429 24.0005 11.9991 24C12.1565 24.0002 12.3121 23.9668 12.4555 23.9019C18.1324 21.5313 21.8191 15.9709 21.793 9.81896ZM11.9992 21.7127C7.30495 19.646 4.30485 14.9691 4.38364 9.84071C4.38364 5.63477 7.79323 2.22518 11.9992 2.22518C16.2051 2.22518 19.6147 5.63477 19.6147 9.84071V9.91152C19.6686 15.0154 16.672 19.6591 11.9992 21.7127Z"
                    fill="#4A3749"
                  />
                </svg></center
              >
            </div>
            <div class="info">
              <h5 class="destination">Dropoff - Villa Mella #4</h5>
              <p>Sent at 08:23 AM</p>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</div>
