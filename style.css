const BASE_URL = 'https://proweb.leoproti.com.br/alunos';

const form = document.getElementById('aluno-form');
const nomeInput = document.getElementById('nome');
const turmaInput = document.getElementById('turma');
const cursoInput = document.getElementById('curso');
const matriculaInput = document.getElementById('matricula');
const alunoIdInput = document.getElementById('aluno-id');
const formTitle = document.getElementById('form-title');
const btnCancel = document.getElementById('btn-cancel');
const formError = document.getElementById('form-error');

const alunosTbody = document.getElementById('alunos-tbody');
const tableMsg = document.getElementById('table-msg');

document.addEventListener('DOMContentLoaded', () => {
  fetchAllAlunos();
});

btnCancel.addEventListener('click', resetForm);

form.addEventListener('submit', async (e) => {
  e.preventDefault();
  clearError();

  const aluno = {
    nome: nomeInput.value.trim(),
    turma: turmaInput.value.trim(),
    curso: cursoInput.value.trim(),
    matricula: matriculaInput.value.trim()
  };

  const err = validateAluno(aluno);
  if (err) return showError(err);

  const id = alunoIdInput.value;
  try {
    if (id) {
      await updateAluno(id, aluno);
    } else {
      await createAluno(aluno);
    }
    resetForm();
    await fetchAllAlunos();
  } catch (error) {
    showError('Erro ao salvar: ' + (error.message || error));
  }
});

function validateAluno(a){
  if(!a.nome) return 'Nome é obrigatório.';
  if(a.nome.length < 3) return 'Nome muito curto.';
  if(!a.turma) return 'Turma é obrigatória.';
  if(!a.curso) return 'Curso é obrigatório.';
  if(!a.matricula) return 'Matrícula é obrigatória.';
  if(a.matricula.length < 3) return 'Matrícula muito curta.';
  return null;
}

function showError(msg){
  formError.textContent = msg;
}

function clearError(){
  formError.textContent = '';
}


async function fetchAllAlunos(){
  alunosTbody.innerHTML = '';
  tableMsg.textContent = 'Carregando...';
  try {
    const res = await fetch(BASE_URL);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    const alunos = await res.json();
    renderAlunosTable(alunos);
    tableMsg.textContent = '';
    if (!alunos || alunos.length === 0) tableMsg.textContent = 'Nenhum aluno encontrado.';
  } catch (err) {
    tableMsg.textContent = 'Erro ao carregar alunos: ' + err.message;
  }
}

async function createAluno(aluno){
  const res = await fetch(BASE_URL, {
    method: 'POST',
    headers: {'Content-Type':'application/json'},
    body: JSON.stringify(aluno)
  });
  if (!res.ok) {
    const text = await res.text();
    throw new Error(`POST falhou: ${res.status} ${text}`);
  }
  return res.json();
}

async function updateAluno(id, aluno){
  const res = await fetch(`${BASE_URL}/${id}`, {
    method: 'PUT',
    headers: {'Content-Type':'application/json'},
    body: JSON.stringify(aluno)
  });
  if (!res.ok) {
    const text = await res.text();
    throw new Error(`PUT falhou: ${res.status} ${text}`);
  }
  return res.json();
}

async function deleteAluno(id){
  const confirmed = confirm('Deseja realmente excluir este aluno?');
  if (!confirmed) return false;
  const res = await fetch(`${BASE_URL}/${id}`, { method: 'DELETE' });
  if (!res.ok) {
    const text = await res.text();
    throw new Error(`DELETE falhou: ${res.status} ${text}`);
  }
  return true;
}


function renderAlunosTable(alunos){
  alunosTbody.innerHTML = '';
  if(!Array.isArray(alunos)) alunos = [];

  alunos.forEach(a => {
    const tr = document.createElement('tr');

    const tdId = document.createElement('td');
    tdId.textContent = a.id ?? '';
    tdId.setAttribute('data-label', 'ID');

    const tdNome = document.createElement('td');
    tdNome.textContent = a.nome ?? '';
    tdNome.setAttribute('data-label', 'Nome');

    const tdTurma = document.createElement('td');
    tdTurma.textContent = a.turma ?? '';
    tdTurma.setAttribute('data-label', 'Turma');

    const tdCurso = document.createElement('td');
    tdCurso.textContent = a.curso ?? '';
    tdCurso.setAttribute('data-label', 'Curso');

    const tdMat = document.createElement('td');
    tdMat.textContent = a.matricula ?? '';
    tdMat.setAttribute('data-label', 'Matrícula');

    const tdAcoes = document.createElement('td');
    tdAcoes.setAttribute('data-label', 'Ações');

    const btnEdit = document.createElement('button');
    btnEdit.textContent = 'Editar';
    btnEdit.className = 'btn-edit small';
    btnEdit.addEventListener('click', () => fillFormForEdit(a));

    const btnDel = document.createElement('button');
    btnDel.textContent = 'Excluir';
    btnDel.className = 'btn-delete small';
    btnDel.addEventListener('click', async () => {
      try {
        const ok = await deleteAluno(a.id);
        if (ok) {
          await fetchAllAlunos();
        }
      } catch (err) {
        alert('Erro ao excluir: ' + err.message);
      }
    });

    const actionsDiv = document.createElement('div');
    actionsDiv.className = 'actions';
    actionsDiv.appendChild(btnEdit);
    actionsDiv.appendChild(btnDel);

    tdAcoes.appendChild(actionsDiv);

    tr.appendChild(tdId);
    tr.appendChild(tdNome);
    tr.appendChild(tdTurma);
    tr.appendChild(tdCurso);
    tr.appendChild(tdMat);
    tr.appendChild(tdAcoes);

    alunosTbody.appendChild(tr);
  });
}

function fillFormForEdit(aluno){
  alunoIdInput.value = aluno.id;
  nomeInput.value = aluno.nome || '';
  turmaInput.value = aluno.turma || '';
  cursoInput.value = aluno.curso || '';
  matriculaInput.value = aluno.matricula || '';
  formTitle.textContent = 'Editar Aluno';
  btnCancel.style.display = 'inline-block';
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function resetForm(){
  alunoIdInput.value = '';
  nomeInput.value = '';
  turmaInput.value = '';
  cursoInput.value = '';
  matriculaInput.value = '';
  formTitle.textContent = 'Cadastrar Aluno';
  clearError();
  btnCancel.style.display = 'none';
}
