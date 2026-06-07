<script>
  const App = {
    state: {
      page: 'dashboard',
      init: null,
      tasks: [],
      dashboard: null,
      today: null,
      planner: null,
      reports: null,
      journal: null,
      selectedRating: 3,
      finishDate: '',
      plannerTab: 'daily',
      viewCache: {},
      cacheTtl: 45000
    },
    days: [
      ['SUN', 'Sun'], ['MON', 'Mon'], ['TUE', 'Tue'], ['WED', 'Wed'],
      ['THU', 'Thu'], ['FRI', 'Fri'], ['SAT', 'Sat']
    ],
    titles: {
      dashboard: 'Dashboard',
      planner: 'Weekly Planner',
      today: 'Today',
      journal: 'Journal',
      reports: 'Reports'
    }
  };

  document.addEventListener('DOMContentLoaded', () => {
    bindChrome();
    buildStaticControls();
    boot();
  });

  function gas(functionName, payload) {
    return new Promise((resolve, reject) => {
      const timeout = window.setTimeout(() => {
        reject(new Error(`${functionName} did not respond within 20 seconds. Check Apps Script Executions for the server error.`));
      }, 20000);
      const runner = google.script.run
        .withSuccessHandler(result => {
          window.clearTimeout(timeout);
          resolve(result);
        })
        .withFailureHandler(error => {
          window.clearTimeout(timeout);
          reject(error);
        });
      if (payload === undefined) runner[functionName]();
      else runner[functionName](payload);
    });
  }

  async function boot() {
    setBusy(true);
    try {
      document.getElementById('dashboardPage').innerHTML = loadingState('Loading your workspace...');
      const initial = await gas('getInitialData');
      App.state.init = initial.init;
      App.state.dashboard = initial.dashboard;
      App.state.planner = initial.planner;
      App.state.today = initial.today;
      App.state.journal = initial.journal;
      App.state.reports = initial.reports;
      App.state.tasks = initial.planner ? initial.planner.tasks : [];
      document.getElementById('todayLabel').textContent = App.state.init.today;
      await loadPage('dashboard', true);
    } catch (error) {
      toast(error.message || error);
      renderError(error);
    } finally {
      setBusy(false);
    }
  }

  function bindChrome() {
    document.getElementById('tabs').addEventListener('click', event => {
      const tab = event.target.closest('[data-page]');
      if (tab) loadPage(tab.dataset.page, true);
    });
    document.getElementById('fab').addEventListener('click', () => openTaskModal());
    document.getElementById('themeToggle').addEventListener('click', toggleTheme);
    document.querySelectorAll('[data-close-modal]').forEach(button => button.addEventListener('click', closeTaskModal));
    document.querySelectorAll('[data-close-finish]').forEach(button => button.addEventListener('click', closeFinishModal));
    document.getElementById('taskForm').addEventListener('submit', saveTaskFromForm);
    document.getElementById('taskForm').elements.type.addEventListener('change', event => updateRepeatControls(event.target.value));
    document.getElementById('finishForm').addEventListener('submit', submitFinishDay);

    const savedTheme = localStorage.getItem('pos-theme');
    if (savedTheme) document.documentElement.dataset.theme = savedTheme;
  }

  function buildStaticControls() {
    const categories = ['Religion', 'Health', 'Learning', 'Work', 'Family', 'Finance', 'Personal'];
    document.getElementById('categorySelect').innerHTML = categories.map(category => `<option>${category}</option>`).join('');
    document.getElementById('repeatPicker').innerHTML = App.days.map(day =>
      `<button type="button" class="chip" data-repeat-day="${day[0]}">${day[1]}</button>`
    ).join('');
    document.getElementById('repeatPicker').addEventListener('click', event => {
      const chip = event.target.closest('[data-repeat-day]');
      if (chip && !chip.disabled) chip.classList.toggle('active');
    });
    document.getElementById('ratingPicker').innerHTML = [1, 2, 3, 4, 5].map(value =>
      `<button type="button" class="chip ${value === 3 ? 'active' : ''}" data-rating="${value}">${value}</button>`
    ).join('');
    document.getElementById('ratingPicker').addEventListener('click', event => {
      const chip = event.target.closest('[data-rating]');
      if (!chip) return;
      App.state.selectedRating = Number(chip.dataset.rating);
      document.querySelectorAll('[data-rating]').forEach(item => item.classList.toggle('active', item === chip));
    });
  }

  async function loadPage(page, preferState = false) {
    App.state.page = page;
    document.getElementById('pageTitle').textContent = App.titles[page];
    document.querySelectorAll('.tab').forEach(tab => tab.classList.toggle('active', tab.dataset.page === page));
    document.querySelectorAll('.page').forEach(section => section.classList.toggle('active', section.id === `${page}Page`));
    if (preferState && renderPageFromState(page)) return;
    activePage().innerHTML = loadingState(`Loading ${App.titles[page]}...`);
    setBusy(true);
    try {
      if (page === 'dashboard') {
        App.state.dashboard = await getCachedView('dashboard', () => gas('getDashboardData'));
        renderDashboard();
      }
      if (page === 'planner') {
        App.state.planner = await getCachedView('planner', () => gas('getWeeklyPlan'));
        App.state.tasks = App.state.planner.tasks;
        renderPlanner();
      }
      if (page === 'today') {
        App.state.today = await getCachedView('today', () => gas('getTodayTasks'));
        renderToday();
      }
      if (page === 'journal') {
        App.state.journal = await getCachedView('journal:month:', () => gas('getJournalEntries', { range: 'month', search: '' }));
        renderJournal();
      }
      if (page === 'reports') {
        App.state.reports = await getCachedView('reports:30', () => gas('getReports', { days: 30 }));
        renderReports();
      }
    } catch (error) {
      renderError(error);
      toast(error.message || error);
    } finally {
      setBusy(false);
    }
  }

  function renderPageFromState(page) {
    if (page === 'dashboard' && App.state.dashboard) return renderDashboard(), true;
    if (page === 'planner' && App.state.planner) return renderPlanner(), true;
    if (page === 'today' && App.state.today) return renderToday(), true;
    if (page === 'journal' && App.state.journal) return renderJournal(), true;
    if (page === 'reports' && App.state.reports) return renderReports(), true;
    return false;
  }

  async function getCachedView(key, loader) {
    const cached = App.state.viewCache[key];
    if (cached && Date.now() - cached.createdAt < App.state.cacheTtl) return cached.value;
    const value = await loader();
    App.state.viewCache[key] = { value, createdAt: Date.now() };
    return value;
  }

  function invalidateClientCache() {
    App.state.viewCache = {};
    App.state.tasks = [];
    App.state.dashboard = null;
    App.state.planner = null;
    App.state.today = null;
    App.state.reports = null;
    App.state.journal = null;
  }

  function renderDashboard() {
    const data = App.state.dashboard;
    document.getElementById('dashboardPage').innerHTML = `
      <div class="card">
        <div class="progress-ring" style="--progress:${data.progress}"><b>${data.progress}%</b></div>
        <p class="muted">${data.completedToday} of ${data.totalToday} items complete today</p>
        <div class="button-row">
          <button class="primary-button" data-go="today">Open Today</button>
          <button class="secondary-button" data-go="planner">Plan Week</button>
        </div>
      </div>
      <div class="stats-grid section-title">
        ${statCard('Current Streak', data.currentStreak)}
        ${statCard('Best Streak', data.bestStreak)}
        ${statCard('Habit Rate', `${data.habitCompletionRate}%`)}
        ${statCard('Completed', data.completedToday)}
      </div>
      <div class="cards-grid">
        <div>
          <div class="section-title"><h2>Today&apos;s Tasks</h2></div>
          <div class="grid">${taskList(data.tasks.filter(task => task.status !== 'done'))}</div>
        </div>
        <div>
          <div class="section-title"><h2>Completed</h2></div>
          <div class="grid">${taskList(data.tasks.filter(task => task.status === 'done'))}</div>
        </div>
      </div>
    `;
    document.getElementById('dashboardPage').querySelectorAll('[data-go]').forEach(button =>
      button.addEventListener('click', () => loadPage(button.dataset.go))
    );
  }

  function renderPlanner() {
    const planner = App.state.planner;
    const tasksById = Object.fromEntries(planner.tasks.map(task => [task.id, task]));
    const rootTasks = planner.tasks.filter(task => task.active !== 'false' && !task.parentId);
    const childrenByParent = groupItems(planner.tasks.filter(task => task.parentId), 'parentId');
    const rootTaskIds = new Set(rootTasks.map(task => task.id));
    const plansByDate = Object.fromEntries(planner.weekDates.map(day => [day.date, []]));
    planner.plans.forEach(plan => {
      if (!plansByDate[plan.date]) return;
      const task = tasksById[plan.taskId];
      if (!task) return;
      const rootId = task.parentId || task.id;
      if (!rootTaskIds.has(rootId)) return;
      const alreadyShown = plansByDate[plan.date].some(item => item.displayTaskId === rootId);
      if (!alreadyShown) plansByDate[plan.date].push(Object.assign({}, plan, { displayTaskId: rootId }));
    });
    const tabTasks = getPlannerTabTasks(App.state.plannerTab, rootTasks, planner.backlog);

    document.getElementById('plannerPage').innerHTML = `
      <div class="planner-workspace">
        <div class="planner-task-tabs" role="tablist" aria-label="Planner task filters">
          ${plannerTabButton('daily', 'Daily', rootTasks.filter(task => task.type === 'daily').length)}
          ${plannerTabButton('repeated', 'Repeated', rootTasks.filter(task => task.type === 'repeated').length)}
          ${plannerTabButton('tasks', 'Tasks', rootTasks.filter(task => task.type === 'oneTime').length)}
          ${plannerTabButton('backlog', 'Backlog', planner.backlog.filter(task => !task.parentId).length)}
        </div>
        <section class="card planner-task-panel">
          <div class="section-title">
            <h2>${plannerTabTitle(App.state.plannerTab)}</h2>
            <button class="tiny-button" data-new-task>New</button>
          </div>
          <div class="grid">
            ${tabTasks.map(task => plannerTaskCard(task, planner.weekDates, childrenByParent[task.id] || [])).join('') || empty(`No ${plannerTabTitle(App.state.plannerTab).toLowerCase()} yet`)}
          </div>
        </section>
        <section class="week-scroll">
          ${planner.weekDates.map(day => `
            <div class="card day-column">
              <h3><span>${day.label}</span><small class="muted">${day.date}</small></h3>
              <div class="drop-zone">
                ${(plansByDate[day.date] || []).map(plan => plannerDayCard(plan, tasksById[plan.displayTaskId], childrenByParent[plan.displayTaskId] || [])).join('') || empty('Open space')}
              </div>
            </div>
          `).join('')}
        </section>
      </div>
    `;

    document.querySelectorAll('[data-planner-tab]').forEach(button => {
      button.addEventListener('click', () => {
        App.state.plannerTab = button.dataset.plannerTab;
        renderPlanner();
      });
    });
    document.querySelectorAll('[data-assign-task]').forEach(select => {
      select.addEventListener('change', async () => {
        if (!select.value) return;
        await gas('saveWeeklyPlan', { taskId: select.dataset.assignTask, date: select.value, status: 'planned' });
        invalidateClientCache();
        toast('Planned');
        App.state.planner = await gas('getWeeklyPlan', planner.weekStart);
        renderPlanner();
      });
    });
    document.querySelectorAll('[data-edit-task]').forEach(button => {
      button.addEventListener('click', () => openTaskModal(planner.tasks.find(task => task.id === button.dataset.editTask)));
    });
    document.querySelectorAll('[data-keep-task]').forEach(button => {
      button.addEventListener('click', () => toast('Kept for this week'));
    });
    document.querySelectorAll('[data-archive-task]').forEach(button => {
      button.addEventListener('click', async () => {
        const task = planner.tasks.find(item => item.id === button.dataset.archiveTask);
        await gas('saveTask', Object.assign({}, task, { active: false }));
        invalidateClientCache();
        toast('Task archived');
        App.state.planner = await gas('getWeeklyPlan', planner.weekStart);
        renderPlanner();
      });
    });
    document.querySelectorAll('[data-plan-status]').forEach(button => {
      button.addEventListener('click', async () => {
        const note = button.dataset.status === 'done' ? promptTaskNote(button.dataset.taskTitle || 'task') : '';
        await gas('saveTaskStatus', { planId: button.dataset.planStatus, status: button.dataset.status, note });
        invalidateClientCache();
        App.state.planner = await gas('getWeeklyPlan', planner.weekStart);
        renderPlanner();
      });
    });
    document.querySelector('[data-new-task]')?.addEventListener('click', () => openTaskModal({ type: defaultTaskTypeForPlannerTab() }));
  }

  function renderToday() {
    const today = App.state.today;
    document.getElementById('todayPage').innerHTML = `
      <div class="section-title"><h2>Daily</h2></div>
      <div class="grid">${todayGroupRows(today.daily || today.habits || [], 'No daily tasks planned')}</div>
      <div class="section-title"><h2>Repeated</h2></div>
      <div class="grid">${todayGroupRows(today.repeated || [], 'No repeated tasks today')}</div>
      <div class="section-title"><h2>Tasks</h2></div>
      <div class="grid">${todayGroupRows(today.tasks || [], 'No one-time tasks for today')}</div>
      <div class="section-title">
        <button class="primary-button" id="finishDayButton">Finish Day</button>
      </div>
    `;

    document.querySelectorAll('[data-habit]').forEach(input => {
      input.addEventListener('change', async () => {
        const note = input.checked ? promptTaskNote(input.dataset.habit) : '';
        await gas('saveHabit', { date: today.date, habit: input.dataset.habit, done: input.checked, note });
        invalidateClientCache();
        App.state.today = await gas('getTodayTasks');
        renderToday();
        toast(input.checked ? 'Marked done' : 'Marked planned');
      });
    });
    document.querySelectorAll('[data-today-status]').forEach(button => {
      button.addEventListener('click', async () => {
        const note = button.dataset.status === 'done' ? promptTaskNote(button.dataset.taskTitle || 'task') : '';
        await gas('saveTaskStatus', { planId: button.dataset.todayStatus, status: button.dataset.status, date: today.date, note });
        invalidateClientCache();
        App.state.today = await gas('getTodayTasks');
        renderToday();
      });
    });
    document.getElementById('finishDayButton').addEventListener('click', openFinishModal);
  }

  function renderJournal() {
    document.getElementById('journalPage').innerHTML = `
      <div class="card grid">
        <label>Search<input id="journalSearch" placeholder="Search journal"></label>
        <div class="button-row">
          <button class="secondary-button" data-journal-range="week">Week</button>
          <button class="secondary-button" data-journal-range="month">Month</button>
        </div>
      </div>
      <div class="section-title"><h2>Entries</h2></div>
      <div class="grid" id="journalList">${journalList(App.state.journal)}</div>
    `;
    document.querySelectorAll('[data-journal-range]').forEach(button => button.addEventListener('click', () => refreshJournal(button.dataset.journalRange)));
    document.getElementById('journalSearch').addEventListener('input', debounce(() => refreshJournal('month'), 300));
  }

  async function refreshJournal(range) {
    const search = document.getElementById('journalSearch').value;
    App.state.journal = await getCachedView(`journal:${range}:${search}`, () => gas('getJournalEntries', { range, search }));
    document.getElementById('journalList').innerHTML = journalList(App.state.journal);
  }

  function renderReports() {
    const reports = App.state.reports;
    document.getElementById('reportsPage').innerHTML = `
      <div class="stats-grid">
        ${statCard('Current Streak', reports.streaks.current)}
        ${statCard('Best Streak', reports.streaks.best)}
        ${statCard('Habit Rate', `${reports.habitCompletionRate}%`)}
        ${statCard('Completed', reports.taskCounts.completed)}
      </div>
      <div class="cards-grid">
        <div class="card">
          <div class="section-title"><h2>Habits</h2></div>
          <div class="grid">${reports.habitStats.map(progressRow).join('') || empty('No habit data yet')}</div>
        </div>
        <div class="card">
          <div class="section-title"><h2>Tasks</h2></div>
          <div class="grid">
            ${progressRow({ habit: 'Completed', percent: reports.taskCounts.completed })}
            ${progressRow({ habit: 'Postponed', percent: reports.taskCounts.postponed })}
            ${progressRow({ habit: 'Cancelled', percent: reports.taskCounts.cancelled })}
          </div>
        </div>
        <div class="card">
          <div class="section-title"><h2>Categories</h2></div>
          <div class="grid">${reports.categoryPerformance.map(item => progressRow({ habit: item.category, percent: item.percent })).join('')}</div>
        </div>
      </div>
    `;
  }

  function statCard(label, value) {
    return `<div class="card stat"><span>${label}</span><strong>${value}</strong></div>`;
  }

  function taskList(tasks) {
    return tasks.map(task => `
      <div class="task-row">
        <span class="category-pill">${task.category || 'Personal'}</span>
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>${task.type || task.status || ''}</small></div>
        <span class="status-pill status-${task.status}">${task.status || 'planned'}</span>
      </div>
    `).join('') || empty('Nothing here');
  }

  function plannerTabButton(tab, label, count) {
    const active = App.state.plannerTab === tab ? 'active' : '';
    return `<button class="planner-tab ${active}" data-planner-tab="${tab}" type="button"><span>${label}</span><b>${count}</b></button>`;
  }

  function plannerTabTitle(tab) {
    return {
      daily: 'Daily Tasks',
      repeated: 'Repeated Tasks',
      tasks: 'Tasks',
      backlog: 'Backlog'
    }[tab] || 'Tasks';
  }

  function defaultTaskTypeForPlannerTab() {
    return App.state.plannerTab === 'daily' ? 'daily' :
      App.state.plannerTab === 'repeated' ? 'repeated' : 'oneTime';
  }

  function getPlannerTabTasks(tab, rootTasks, backlog) {
    if (tab === 'daily') return rootTasks.filter(task => task.type === 'daily');
    if (tab === 'repeated') return rootTasks.filter(task => task.type === 'repeated');
    if (tab === 'backlog') return backlog.filter(task => !task.parentId);
    return rootTasks.filter(task => task.type === 'oneTime');
  }

  function plannerTaskCard(task, weekDates, children) {
    const childText = children.length ? `${children.length} subtasks` : task.category;
    const meta = task.type === 'repeated' ? `Pattern: ${patternLabel(task.repeatPattern)}` : childText;
    const assignment = task.type === 'oneTime' ? `
      <select data-assign-task="${task.id}">
        <option value="">Assign day</option>
        ${weekDates.map(day => `<option value="${day.date}">${day.shortLabel} ${day.date}</option>`).join('')}
      </select>
    ` : '';

    return `
      <div class="planner-item">
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>${escapeHtml(meta)}</small></div>
        ${children.length ? `<div class="subtask-line">${children.map(child => escapeHtml(child.title)).join(' · ')}</div>` : ''}
        <div class="button-row">
          <button class="tiny-button" data-edit-task="${task.id}">Edit</button>
          <button class="tiny-button" data-archive-task="${task.id}">Archive</button>
        </div>
        ${assignment}
      </div>
    `;
  }

  function repeatedTaskCard(task) {
    return `
      <div class="planner-item">
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>Current Pattern: ${patternLabel(task.repeatPattern)}</small></div>
        <div class="button-row">
          <button class="tiny-button" data-edit-task="${task.id}">Edit</button>
          <button class="tiny-button" data-keep-task="${task.id}">Keep</button>
        </div>
      </div>
    `;
  }

  function backlogTaskCard(task, weekDates) {
    return `
      <div class="planner-item">
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>${task.category}</small></div>
        <div class="button-row"><button class="tiny-button" data-edit-task="${task.id}">Edit</button></div>
        <select data-assign-task="${task.id}">
          <option value="">Assign day</option>
          ${weekDates.map(day => `<option value="${day.date}">${day.shortLabel} ${day.date}</option>`).join('')}
        </select>
      </div>
    `;
  }

  function taskLibraryCard(task) {
    return `
      <div class="planner-item">
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>${task.type} · ${task.category}</small></div>
        <div class="button-row">
          <button class="tiny-button" data-edit-task="${task.id}">Edit</button>
          <button class="tiny-button" data-archive-task="${task.id}">Archive</button>
        </div>
      </div>
    `;
  }

  function plannerDayCard(plan, task, children = []) {
    if (!task) return '';
    const meta = `${task.category} · ${task.type}${children.length ? ` · ${children.length} subtasks` : ''}`;
    const actions = children.length ? '' : `
      <div class="button-row">
        <button class="tiny-button" data-plan-status="${plan.id}" data-status="done">Done</button>
        <button class="tiny-button" data-plan-status="${plan.id}" data-status="postponed">Postpone</button>
      </div>
    `;
    return `
      <div class="planner-item">
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>${escapeHtml(meta)}</small></div>
        ${actions}
      </div>
    `;
  }

  function plannerPlanCard(plan, task) {
    if (!task) return '';
    return `
      <div class="planner-item">
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>${task.category} · ${task.type}</small></div>
        <div class="button-row">
          <button class="tiny-button" data-plan-status="${plan.id}" data-status="done" data-task-title="${escapeAttr(task.title)}">Done</button>
          <button class="tiny-button" data-plan-status="${plan.id}" data-status="postponed">Postpone</button>
        </div>
      </div>
    `;
  }

  function habitRow(habit) {
    return `
      <label class="habit-row">
        <input class="check" type="checkbox" data-habit="${escapeAttr(habit.title)}" ${habit.done ? 'checked' : ''}>
        <span class="task-main"><b>${escapeHtml(habit.title)}</b><br><small>${habit.category}</small>${habit.note ? `<br><small class="task-note">${escapeHtml(habit.note)}</small>` : ''}</span>
        <span class="status-pill ${habit.done ? 'status-done' : ''}">${habit.done ? 'done' : 'planned'}</span>
      </label>
    `;
  }

  function todayTaskRow(task) {
    return `
      <div class="task-row">
        <span class="category-pill">${task.category}</span>
        <div class="task-main"><b>${escapeHtml(task.title)}</b><br><small>${task.type}</small>${task.note ? `<br><small class="task-note">${escapeHtml(task.note)}</small>` : ''}</div>
        <div class="button-row">
          <button class="tiny-button" data-today-status="${task.planId}" data-status="done" data-task-title="${escapeAttr(task.title)}">Done</button>
          <button class="tiny-button" data-today-status="${task.planId}" data-status="postponed">Later</button>
        </div>
      </div>
    `;
  }

  function todayGroupRows(items, emptyText) {
    return items.map(item => item.children && item.children.length ? parentTaskRow(item) :
      (item.type === 'daily' ? habitRow(item) : todayTaskRow(item))
    ).join('') || empty(emptyText);
  }

  function parentTaskRow(item) {
    return `
      <div class="planner-item">
        <div class="task-main"><b>${escapeHtml(item.title)}</b><br><small>${item.category} · ${item.children.filter(child => child.done).length}/${item.children.length}</small></div>
        <div class="grid">${item.children.map(child => child.type === 'daily' ? habitRow(child) : todayTaskRow(child)).join('')}</div>
      </div>
    `;
  }

  function journalList(entries) {
    return entries.map(entry => `
      <article class="journal-entry">
        <div class="section-title"><h3>${entry.date}</h3><span class="status-pill">${entry.score}/5</span></div>
        <p>${escapeHtml(entry.journal || 'No journal text')}</p>
      </article>
    `).join('') || empty('No journal entries yet');
  }

  function progressRow(item) {
    const label = item.habit || item.category;
    const value = Number(item.percent || 0);
    return `
      <div>
        <div class="section-title"><span>${escapeHtml(label)}</span><b>${value}%</b></div>
        <div class="bar" style="--value:${Math.min(100, value)}%"><span></span></div>
      </div>
    `;
  }

  async function openTaskModal(task = {}) {
    const form = document.getElementById('taskForm');
    await populateParentTaskSelect(task);
    form.reset();
    form.elements.id.value = task.id || '';
    form.elements.title.value = task.title || '';
    form.elements.type.value = task.type || 'oneTime';
    form.elements.category.value = task.category || 'Personal';
    form.elements.parentId.value = task.parentId || '';
    form.elements.active.checked = task.active !== 'false';
    const pattern = (task.repeatPattern || '').split(',');
    document.querySelectorAll('[data-repeat-day]').forEach(chip => chip.classList.toggle('active', pattern.includes(chip.dataset.repeatDay)));
    updateRepeatControls(form.elements.type.value);
    document.getElementById('taskModal').classList.remove('hidden');
  }

  function updateRepeatControls(type) {
    const enabled = type === 'repeated';
    const fieldset = document.getElementById('repeatFieldset');
    fieldset.classList.toggle('disabled-field', !enabled);
    document.querySelectorAll('[data-repeat-day]').forEach(chip => {
      chip.disabled = !enabled;
      if (!enabled) chip.classList.remove('active');
    });
  }

  async function populateParentTaskSelect(task = {}) {
    if (!App.state.tasks || !App.state.tasks.length) {
      try {
        App.state.tasks = await gas('getTasks');
      } catch (error) {
        App.state.tasks = [];
      }
    }
    const options = (App.state.tasks || [])
      .filter(item => item.id !== task.id && item.active !== 'false' && !item.parentId)
      .map(item => `<option value="${escapeAttr(item.id)}">${escapeHtml(item.title)} (${item.type})</option>`)
      .join('');
    document.getElementById('parentTaskSelect').innerHTML = `<option value="">None</option>${options}`;
  }

  function closeTaskModal() {
    document.getElementById('taskModal').classList.add('hidden');
  }

  async function saveTaskFromForm(event) {
    event.preventDefault();
    const form = event.currentTarget;
    const repeatPattern = Array.from(document.querySelectorAll('[data-repeat-day].active')).map(chip => chip.dataset.repeatDay).join(',');
    const task = {
      id: form.elements.id.value,
      title: form.elements.title.value,
      type: form.elements.type.value,
      category: form.elements.category.value,
      parentId: form.elements.parentId.value,
      repeatPattern,
      active: form.elements.active.checked
    };
    await gas('saveTask', task);
    invalidateClientCache();
    closeTaskModal();
    toast('Task saved');
    await loadPage(App.state.page);
  }

  function openFinishModal() {
    const today = App.state.today;
    if (!today || !today.date) {
      toast('Today data is still loading. Open Today again and retry.');
      loadPage('today');
      return;
    }
    App.state.finishDate = today.date;
    const incomplete = flattenTodayItems(today).filter(task => task.planId && task.status !== 'done' && !task.done);
    document.getElementById('finishTasks').innerHTML = incomplete.map(task => `
      <div class="planner-item">
        <b>${escapeHtml(task.title)}</b>
        <select data-finish-plan="${task.planId}">
          <option value="tomorrow">Move To Tomorrow</option>
          <option value="done">Done</option>
          <option value="postpone">Postpone</option>
          <option value="cancel">Cancel</option>
          <option value="reschedule">Reschedule</option>
        </select>
        <input type="date" data-new-date="${task.planId}" class="hidden">
      </div>
    `).join('') || empty('All tasks are complete');
    document.querySelectorAll('[data-finish-plan]').forEach(select => {
      select.addEventListener('change', () => {
        const dateInput = document.querySelector(`[data-new-date="${select.dataset.finishPlan}"]`);
        dateInput.classList.toggle('hidden', select.value !== 'reschedule');
      });
    });
    document.getElementById('finishModal').classList.remove('hidden');
  }

  function flattenTodayItems(today) {
    const groups = []
      .concat(today.daily || today.habits || [])
      .concat(today.repeated || [])
      .concat(today.tasks || []);
    return groups.flatMap(item => item.children && item.children.length ? item.children : [item]);
  }

  function closeFinishModal() {
    document.getElementById('finishModal').classList.add('hidden');
  }

  async function submitFinishDay(event) {
    event.preventDefault();
    const form = event.currentTarget;
    const submitButton = form.querySelector('button[type="submit"]');
    const date = App.state.finishDate || (App.state.today && App.state.today.date) || App.state.init.today;
    const decisions = Array.from(document.querySelectorAll('[data-finish-plan]')).map(select => ({
      planId: select.dataset.finishPlan,
      action: select.value,
      newDate: document.querySelector(`[data-new-date="${select.dataset.finishPlan}"]`).value
    }));
    submitButton.disabled = true;
    submitButton.textContent = 'Saving...';
    try {
      await gas('finishDay', {
        date,
        decisions,
        score: App.state.selectedRating,
        journal: form.journal.value
      });
      invalidateClientCache();
      closeFinishModal();
      toast('Day reviewed');
      await loadPage('dashboard');
    } catch (error) {
      toast(error.message || error);
    } finally {
      submitButton.disabled = false;
      submitButton.textContent = 'Save Review';
    }
  }

  function patternLabel(pattern) {
    if (!pattern) return 'No days selected';
    const labels = Object.fromEntries(App.days);
    return pattern.split(',').map(day => labels[day] || day).join(' ');
  }

  function empty(text) {
    return `<div class="empty-state">${text}</div>`;
  }

  function loadingState(text) {
    return `
      <div class="empty-state loading-state">
        <span class="spinner" aria-hidden="true"></span>
        <span>${escapeHtml(text)}</span>
      </div>
    `;
  }

  function promptTaskNote(title) {
    const value = window.prompt(`Add note for ${title} (optional)`, '');
    return value === null ? '' : value.trim();
  }

  function toast(message) {
    const node = document.getElementById('toast');
    node.textContent = message;
    node.classList.remove('hidden');
    window.clearTimeout(toast.timer);
    toast.timer = window.setTimeout(() => node.classList.add('hidden'), 2600);
  }

  function activePage() {
    return document.getElementById(`${App.state.page}Page`);
  }

  function renderError(error) {
    const message = error && error.message ? error.message : String(error || 'Unknown error');
    activePage().innerHTML = `
      <div class="card">
        <h2>Could not load ${App.titles[App.state.page]}</h2>
        <p class="muted">${escapeHtml(message)}</p>
        <button class="primary-button" id="retryPage">Retry</button>
      </div>
    `;
    document.getElementById('retryPage').addEventListener('click', () => loadPage(App.state.page));
  }

  function setBusy(isBusy) {
    document.body.toggleAttribute('aria-busy', isBusy);
  }

  function toggleTheme() {
    const next = document.documentElement.dataset.theme === 'dark' ? '' : 'dark';
    if (next) document.documentElement.dataset.theme = next;
    else delete document.documentElement.dataset.theme;
    localStorage.setItem('pos-theme', next);
  }

  function debounce(fn, wait) {
    let timer;
    return (...args) => {
      clearTimeout(timer);
      timer = setTimeout(() => fn(...args), wait);
    };
  }

  function groupItems(items, key) {
    return items.reduce((groups, item) => {
      const value = item[key] || '';
      if (!groups[value]) groups[value] = [];
      groups[value].push(item);
      return groups;
    }, {});
  }

  function escapeHtml(value) {
    return String(value || '').replace(/[&<>"']/g, char => ({
      '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#039;'
    }[char]));
  }

  function escapeAttr(value) {
    return escapeHtml(value).replace(/`/g, '&#096;');
  }
</script>
