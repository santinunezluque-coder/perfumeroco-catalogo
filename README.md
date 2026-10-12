          </div>`;
      });
    }
    html += `</div>`;
  });
  el.innerHTML = html;
}

function switchView(view){
  const catBtn = document.querySelector('[data-view="categoria"]');
  const marBtn = document.querySelector('[data-view="marca"]');
  const catView = document.getElementById('categoriaView');
  const marView = document.getElementById('marcaView');
  const catChips = document.getElementById('categoryChips');
  const titleText = document.getElementById('sectionTitleText');

  if (view === 'marca'){
    catBtn.classList.remove('active'); marBtn.classList.add('active');
    catView.style.display='none'; marView.style.display='block';
    catChips.style.display='none';
    titleText.textContent = 'Explorá por Casa';
  } else {
    marBtn.classList.remove('active'); catBtn.classList.add('active');
    marView.style.display='none'; catView.style.display='block';
    catChips.style.display='flex';
    titleText.textContent = 'Explorá por Categoría';
  }
}

document.getElementById('viewSwitch').addEventListener('click', (e) => {
  if (e.target.tagName === 'BUTTON'){
    const view = e.target.dataset.view;
    if (view === 'marca'){
      renderMarcaView('__all__');
    }
    switchView(view);
    document.getElementById('sectionTitle').scrollIntoView({ behavior:'smooth', block:'start' });
  }
});

const scrollTopBtn = document.getElementById('scrollTopBtn');
window.addEventListener('scroll', () => {
  scrollTopBtn.classList.toggle('visible', window.scrollY > 400);
});
scrollTopBtn.addEventListener('click', () => {
  window.scrollTo({ top: 0, behavior: 'smooth' });
});

function openProductModal(idx){
  const p = PRODUCTS[idx];
  if (!p) return;
  const img = p.image || PLACEHOLDER_IMG;
  const notesBlock = p.notes && p.notes.length
    ? `<div class="modal-notes"><b>Notas:</b> ${p.notes.join(', ')}</div>`
    : '';
  const presentations = p.presentations && p.presentations.length ? p.presentations : DEFAULT_PRESENTATIONS;
  const presentationsBlock = `
    <div class="modal-presentations">
      <b>Disponible en:</b>
      <div class="presentation-tags">
        ${presentations.map(slug => {
          const info = PRESENTATION_INFO[slug];
          return info ? `<span class="presentation-tag" title="${info.desc}">${info.label}</span>` : '';
        }).join('')}
      </div>
    </div>`;
  document.getElementById('modalBody').innerHTML = `
    <div class="modal-img"><img src="${img}" alt="${p.name}"></div>
    <div class="modal-info">
      <span class="product-brand">${p.brand}</span>
      <h2 class="modal-name">${p.name}</h2>
      <div class="badge-row">
        <span class="badge badge-gender ${genderClass(p.gender)}">${p.gender}</span>
        ${p.available ? '<span class="badge badge-available">Entrega inmediata</span>' : ''}
        ${p.year ? `<span class="badge">${p.year}</span>` : ''}
        <span class="badge">${p.category}</span>
      </div>
      ${p.description ? `<p class="modal-desc">${p.description}</p>` : ''}
      ${notesBlock}
      ${presentationsBlock}
      <span class="modal-price">${money(p.price)}</span>
    </div>`;
  document.getElementById('productModal').classList.add('open');
  document.body.style.overflow = 'hidden';
}

function closeProductModal(){
  document.getElementById('productModal').classList.remove('open');
  document.body.style.overflow = '';
}

document.getElementById('productModal').addEventListener('click', (e) => {
  if (e.target.id === 'productModal') closeProductModal();
});
document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') closeProductModal();
});

renderBrandCarousel();
renderCategoryChips();
renderCategoriaView('__all__');
renderMarcaView('__all__');
</script>

</body>
</html>
