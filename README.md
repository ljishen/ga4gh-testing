# Continuous Testing for GA4GH API

This project aims at runing `load testing` and `compliance test suite` for GA4GH API, and organizes easy to read testing results for iterative development. It makes use of `Docker` and `Ansible`. The Ansible project is under folder `testing`, the others are the docker images used in the testing process.

#### Usage
- `cd` to folder `testing`
- Install necessary roles for the first time

    ```bash
    ansible-galaxy install -r requirements.yml
    ```
- Run the playbook

    ```bash
    ansible-playbook test.yml -k
    ```
The prompt will ask for connection password.

After testing finished, it will create the following directory structures. All testing results store at `{{ file_server_root }}` (default `/srv/http`).
```
 Server:
   - {{ ga4gh_working_dir }}/srv/data:   For All Server Data
 Master:
   - {{ file_server_root }}/{{ test_results_folder_name }}/server_{{ server_commit_hash }}/{{ locust_folder_name }}/{{ locustfile_hash }}:    For Load Testing Results
   - {{ file_server_root }}/{{ test_results_folder_name }}/server_{{ server_commit_hash }}/{{ compliance_folder_name }}/{{ compliance_test_suite_commit_hash }}:    For Compliance Tests Results
   - {{ ga4gh_working_dir }}/{{ ga4gh_testing_folder_name }}: Clone of this project

 Locust:
   - {{ ga4gh_working_dir }}/{{ locust_folder_name }}/{{ locust_filename }}: For locustfile.py
```

- The HTTP file server is at `http://<master_host>:8085` by default
- The locust web interface is at `http://<master_host>:8089` if you run load testing in distributed mode

---

Please read the plays in playbook file `testing/test.yml` for more details.
