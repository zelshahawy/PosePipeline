# How to Run PosePipeline

## 1. Start the DataJoint MySQL Database

```bash
cd docker/
./run_datajoint.sh start
```

First run pulls the MySQL 8.0 image via Apptainer. Subsequent runs reuse the cached `.sif` file.

Other commands:
```bash
./run_datajoint.sh stop     # stop the database
./run_datajoint.sh status   # check if running
./run_datajoint.sh shell    # drop into the container
```

## 2. Install Dependencies with uv

```bash
cd /project/cmgreenspon/PosePipeline
uv sync
```

For dev/test extras:
```bash
uv pip install -e ".[dev]"
```

## 3. Create the Video Store Directory

```bash
mkdir -p /project/cmgreenspon/PosePipeline/stored_vids
```

This is where DataJoint will store managed video files (configured in `dj_local_conf.json`).

## 4. Run Pipeline Commands

```bash
uv run python -c "from pose_pipeline.pipeline import Video; print(Video())"
```

Or activate the venv:
```bash
source .venv/bin/activate
python -c "from pose_pipeline.pipeline import Video; ..."
```

## 5. Run Tests

```bash
uv run pytest tests/
```

## Notes

- `dj_local_conf.json` must be in the directory you run Python from (or set `DJ_LOCAL_CONF` env var to point to it)
- Videos are inserted into the `Video` table by passing a file path — DataJoint copies them into `stored_vids/`
- The database persists data in `docker/data/`
