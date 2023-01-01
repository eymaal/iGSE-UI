<script>
    let customers=0, unpaidBills=0, eDayUnits, eNightUnits, gasUnits;
</script>

<div class="stats stats-vertical lg:stats-horizontal shadow bg-primary text-primary-content">    
    <div class="stat">
    <div class="stat-title">Customers</div>
    <div class="stat-value">{customers}</div>
    <!-- <div class="stat-desc">Jan 1st - Feb 1st</div> -->
    </div>    
    <div class="stat">
    <div class="stat-title">Pending Bills</div>
    <div class="stat-value">{unpaidBills}</div>
    <div class="stat-desc">↗︎</div>
    </div>    
    <!-- <div class="stat">
    <div class="stat-title">New Registers</div>
    <div class="stat-value">1,200</div>
    <div class="stat-desc">↘︎ 90 (14%)</div>
    </div>     -->
</div>