
uses:
    ci / cd management
    code & repo management

Workflows  ->  Jobs  -> Steps

parallel and sequential jobs:
    needs: job_name

expression and content :
    ${{ toJson(github) }}
    
push:
    branches:
        - main
        - dev-*
    path: 
        - 'some-path'
    path-ignore:
        - 'some-path'

we can cancel the workflow 
we can skip the workflow by adding certain message to the commit message. [skip ci] [skip action]

Job Artifacs:
    storing the artifacts of job(build) and using it in another job(deploy)
    actions/upload-artifact@v3
    actions/download-artifact@v3
    with:
        name:
        path: |
            includepath
            ! excludedpath
Job Outputs:
    creating a variable inside job OR exporting a variable from one step to another step
        run: echo 'key=value' >> $GITHUB_OUTPUT
    exporting the variable from one perticular job to another job
        outputs:
            key = {{ steps.stepId.outputs.outputName }}
        ${{ needs.jobName.outputs.outputName }}

Caching 
    uses: actions/cache@v3
    with:
        path:
        key: name-{{ hashFiles('**/package-lock.json') }}

Environment variables:
    * common to all job
    * specific to job
    * specific to step
    ${{env.name}}
    but will be exposed on code base.

Secrets:
    Repository Secrets: Same fot the entire repo.
    Environment Secrets: Diff for each environement
        We can create env using some name
        create secrets inside the env
        connect this env name with the job by using 
            environment: env_name
        Protection Rule / Branch Restrictions: 
            we can also restrict the branch to access this environments. like if we push to other than the main branch then any jobs that are referencing to the env_name wouldn't be executed if we restrict this env to only main.

            Basically we can restricts the perticular Jobs using this environments.
    {{ secrets.DB_USERNAME }}
    github will hide the value of secret even we print it on actions.

Conditions:
    step level + job level

    failure() -> if any previous one is failed
    success() -> if all previous jobs are success
    cancelled() -> if ay previous one is cancelled

    install dependencies only when cache.hit is not true

Matrix Strategy:

    job:
        strategy:
            matrix:
                node-version: [1,2,3]
                os: ['ubuntu','windows','mac']

                include: combinations
                exclude: combinations

Reusable workflow:
    passing inputs
    passing secrets

    outputs:    step -> job -> workflow -> another workflow
    
name:
on: []
jobs:
    job_name:
        runs-on:
        steps:
            -   name:
                run: 
                shell: 
                ( use actions from another repo )
            -   name: 
                uses:
                with:
                    key: value



release-v23.05.0-3-g4388e32
release-v23.05.0-3-g4388e32-passed-qual
release-v23.05.0-3-g4388e32-passed-cert
CHG0657445