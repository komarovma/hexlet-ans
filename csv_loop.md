<h2 class="code-line" data-line-start=0 data-line-end=1 ><a id="cat_datacsv_0"></a>cat data.csv</h2>
<pre><code>username,password
john.doe,secret123
jane.smith,letmein456
</code></pre>
<h2 class="code-line" data-line-start=6 data-line-end=7 ><a id="cat_rcsvyml_6"></a>cat rcsv.yml</h2>
<pre><code>---
- name: Read CSV file with loop
  hosts: localhost
  gather_facts: no

  vars:
    csv_file: &quot;data.csv&quot;

  tasks:
    - name: Read CSV data
      community.general.read_csv:
        path: &quot;{{ csv_file }}&quot;
        delimiter: &quot;,&quot;
      register: csv_data

    - name: Print CSV data
      include_tasks: mtask.yml
      loop: &quot;{{ csv_data.list }}&quot;
</code></pre>
<h2 class="code-line" data-line-start=27 data-line-end=28 ><a id="mtaskyml_27"></a>mtask.yml</h2>
<pre><code>- name: &quot;Print CSV 0&quot;
  debug:
    msg: &quot;{{ 'USERNAME: ' + item.username  }} &quot;

- name: &quot;Print CSV 1&quot;
  debug:
    msg: &quot;{{ 'PASSWORD: ' + item.password }} &quot;
</code></pre>
<h2 class="code-line" data-line-start=38 data-line-end=39 ><a id="ansibleplaybook_rcsvyml_38"></a>ansible-playbook rcsv.yml</h2>
<pre><code>[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match
'all'
[WARNING]: Collection community.general does not support Ansible version 2.10.8

PLAY [Read CSV file with loop] *****************************************************************************************

TASK [Read CSV data] ***************************************************************************************************
ok: [localhost]

TASK [Print CSV data] **************************************************************************************************
included: /home/mike/ans/mtask.yml for localhost =&gt; (item={'username': 'john.doe', 'password': 'secret123'})
included: /home/mike/ans/mtask.yml for localhost =&gt; (item={'username': 'jane.smith', 'password': 'letmein456'})

TASK [Print CSV 0] *****************************************************************************************************
ok: [localhost] =&gt; {
    &quot;msg&quot;: &quot;USERNAME: john.doe &quot;
}

TASK [Print CSV 1] *****************************************************************************************************
ok: [localhost] =&gt; {
    &quot;msg&quot;: &quot;PASSWORD: secret123 &quot;
}

TASK [Print CSV 0] *****************************************************************************************************
ok: [localhost] =&gt; {
    &quot;msg&quot;: &quot;USERNAME: jane.smith &quot;
}

TASK [Print CSV 1] *****************************************************************************************************
ok: [localhost] =&gt; {
    &quot;msg&quot;: &quot;PASSWORD: letmein456 &quot;
}

PLAY RECAP *************************************************************************************************************
localhost                  : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0</code></pre>
