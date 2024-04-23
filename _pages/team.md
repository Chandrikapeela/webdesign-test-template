---
title: "test page"
layout: gridlay
sitemap: false
permalink: /test/
---

# Group Members  

{% assign ap_members = '' | split: '' %}
{% assign us_members = '' | split: '' %}
{% assign msr_members = '' | split: '' %}
{% assign phd_members = '' | split: '' %}
{% assign ra_members = '' | split: '' %}
{% assign int_members = '' | split: '' %}
{% assign oth_members = '' | split: '' %}

{% assign sorted_members = site.data.team | sort: "year" %}

{% for member in sorted_members %}
{% if member.position == 'Assistant Professor' %}
{% assign ap_members = ap_members | push: member %}
{% elsif member.position == 'Undergraduate student' %}
{% assign us_members = us_members | push: member %}
{% elsif member.position == 'MS(R) student' %}
{% assign msr_members = msr_members | push: member %}
{% elsif member.position == 'PhD student' %}
{% assign phd_members = phd_members | push: member %}
{% elsif member.position == 'Research Assistant' %}
{% assign ra_members = ra_members | push: member %}
{% elsif member.position == 'Intern' %}
{% assign int_members = int_members | push: member %}
{% else %}
{% assign oth_members = oth_members | push: member %}
{% endif %}
{% endfor %}

{% assign sorted_members = '' | split: '' | concat: ap_members | concat: phd_members | concat: msr_members | concat: ra_members | concat: us_members | concat: int_members | concat: oth_members %}




<div id="filter-wrapper" class="filter-wrapper">

</div>

<div id="main-root" class="row">
</div>

<script>
  var sortedMembers = [
    {% for member in sorted_members %}
      {
        name: "{{ member.name }}",
        position: "{{ member.position }}",
        affiliation: "{{ member.affiliation }}",
        alumuni : "{{ member.alumni }}",
        currentAlumuni : "{{ member.alumni_current }}",
        display: "{{ member.display }}",
        year: "{{ member.year }}",
        image: "{{ member.image }}",
        email: "{{ member.email }}",
        bio1: "{{ member.bio1 }}",
        bio2: "{{ member.bio2 }}",
        bio3: "{{ member.bio3 }}",
        bio4: "{{ member.bio4 }}"
      },
    {% endfor %}
  ];

  const mainRoot = document.getElementById('main-root');
  const filterWrapper = document.getElementById('filter-wrapper');
  const filterArray = ['all'];
  const filterOptions = ['all'];
  let filterOptionsInnerHtml = `<div class="filterOption">all</div>`;
  let filterButtons;

    function changeFilter(){
    let innerHtmlString = '';
    for(let i =0; i < sortedMembers.length; i++){
      if(sortedMembers[i].display == 1 && (filterArray.includes(sortedMembers[i].position.toLowerCase().trim()) || filterArray.includes('all'))){
        innerHtmlString +=  `<div class="col-sm-6 clearfix">
        <img src="${sortedMembers[i].image}" class="img-responsive" width="35%" style="float: left" />
        <h4>${sortedMembers[i].name}</h4>
        <i>${sortedMembers[i].position}, ${sortedMembers[i].affiliation} <br>email: ${sortedMembers[i].email}</i>
        <ul style="overflow: hidden">
          <li> {{ member.bio1 }} </li>
          <li> {{ member.bio2 }} </li>
          <li> {{ member.bio3 }} </li>
          <li> {{ member.bio4 }} </li>
        </ul>
      </div>`
      }
      
    }
    mainRoot.innerHTML = innerHtmlString;
  }

  sortedMembers.forEach(ele =>{
    if(!filterOptions.includes(ele.position.toLowerCase().trim()) && ele.position != ""){
          filterOptions.push(ele.position.toLowerCase().trim());
          filterOptionsInnerHtml += `<div class="filterOption">${ele.position.toLowerCase().trim()}</div>`;
    }
  })

  changeFilter();

  setTimeout(() =>{
  filterButtons = document.querySelectorAll('.filterOption');
    filterButtons.forEach(btn =>{
      if(btn.innerText == 'all'){
        btn.classList.add('filter-active');
      }
    btn.addEventListener('click', () =>{
      if(filterArray.includes(btn.innerText)){
      filterArray.splice(filterArray.indexOf(btn.innerText),1);
      btn.classList.remove('filter-active');
      } else {
        filterArray.push(btn.innerText);
        btn.classList.add('filter-active');
      }
      changeFilter();
    })
  })
  },1000)

   

  filterWrapper.innerHTML = filterOptionsInnerHtml;

</script>