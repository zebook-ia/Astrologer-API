This file is a merged representation of the entire codebase, combined into a single document by Repomix.

# File Summary

## Purpose
This file contains a packed representation of the entire repository's contents.
It is designed to be easily consumable by AI systems for analysis, code review,
or other automated processes.

## File Format
The content is organized as follows:
1. This summary section
2. Repository information
3. Directory structure
4. Repository files (if enabled)
5. Multiple file entries, each consisting of:
  a. A header with the file path (## File: path/to/file)
  b. The full contents of the file in a code block

## Usage Guidelines
- This file should be treated as read-only. Any changes should be made to the
  original repository files, not this packed version.
- When processing this file, use the file path to distinguish
  between different files in the repository.
- Be aware that this file may contain sensitive information. Handle it with
  the same level of security as you would the original repository.

## Notes
- Some files may have been excluded based on .gitignore rules and Repomix's configuration
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Files are sorted by Git change count (files with more changes are at the bottom)

# Directory Structure
```
app/
  config/
    __init__.py
    config.dev.toml
    config.prod.toml
    settings.py
  middleware/
    __init__.py
    secret_key_checker_middleware.py
  routers/
    __init__.py
    main_router.py
  tmp/
    README.md
  types/
    request_models.py
    response_models.py
  utils/
    get_ntp_time.py
    get_time_from_google.py
    internal_server_error_json_response.py
    write_request_to_log.py
  __init__.py
  main.py
tests/
  test_main.py
.editorconfig
.gitignore
dump_schema.py
openapi.json
Pipfile
Procfile
README.md
```

# Files

## File: app/config/__init__.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""
````

## File: app/config/config.dev.toml
````toml
admin_email = "kerykeion.astrology@gmail.com"
debug = true
docs_url = "/docs"
redoc_url = "/redoc"
log_level = 10
secret_key_name = "X-RapidAPI-Proxy-Secret"

allowed_hosts = ['*']

allowed_cors_origins = ['*']
````

## File: app/config/config.prod.toml
````toml
admin_email = "kerykeion.astrology@gmail.com"
debug = false
docs_url = "/docs"
redoc_url = "/redoc"
log_level = 20
secret_key_name = "X-RapidAPI-Proxy-Secret"

allowed_hosts = [
    "rapidapi.com",
    "*.rapidapi.com",

    # RapidAPI US East:
    "107.23.255.128",
    "107.23.255.129",
    "107.23.255.131",
    "107.23.255.132",
    "107.23.255.133",
    "107.23.255.134",
    "107.23.255.135",
    "107.23.255.137",
    "107.23.255.138",
    "107.23.255.139",
    "107.23.255.140",
    "107.23.255.141",
    "107.23.255.142",
    "107.23.255.143",
    "107.23.255.144",
    "107.23.255.145",
    "107.23.255.146",
    "107.23.255.147",
    "107.23.255.148",
    "107.23.255.149",
    "107.23.255.150",
    "107.23.255.151",
    "107.23.255.152",
    "107.23.255.153",
    "107.23.255.154",
    "107.23.255.155",
    "107.23.255.156",
    "107.23.255.157",
    "107.23.255.158",
    "107.23.255.159",

    # Rapid Api US West:
    "35.162.152.183",
    "52.38.28.241",
    "52.35.67.149",
    "54.149.215.237",

    # Mumbai:
    "13.127.146.34",
    "13.127.207.241",
    "13.232.235.243",
    "13.233.81.143",

    # Tokio:
    "13.112.233.15",
    "54.250.57.56",
    "18.182.156.77",
    "52.194.200.157",

    # Frankfurt:
    "3.120.160.95",
    "18.184.214.33",
    "18.197.117.10",
    "3.121.144.151",

    # Sydney:
    "13.239.156.114",
    "13.238.1.253",
    "13.54.58.4",
    "54.153.234.158",

    # South America:
    "18.228.167.221",
    "18.228.209.157",
    "18.228.209.53",
    "18.228.69.72",

    # Singapore:
    "13.228.169.5",
    "3.0.35.31",
    "3.1.111.112",
    "52.220.50.179",

    # Ireland:
    "34.250.225.89",
    "52.30.208.221",
    "63.34.177.151",
    "63.35.2.11",
]

allowed_cors_origins = []
````

## File: app/config/settings.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""

import pathlib
from logging import getLogger
from os import getenv
from pydantic_settings import BaseSettings
from tomllib import load as load_toml


logger = getLogger(__name__)

ENV_TYPE = getenv("ENV_TYPE", False)
RAPID_API_SECRET_KEY = getenv("RAPID_API_SECRET_KEY", False)
GEONAMES_USERNAME = getenv("GEONAMES_USERNAME", False)

# Open config file
if ENV_TYPE == "production":
    logger.info("Loading production config")
    with open(pathlib.Path(__file__).parent.absolute() / "config.prod.toml", "rb") as config_file:
        config = load_toml(config_file)

elif ENV_TYPE == "test":
    logger.info("Loading test config")
    with open(pathlib.Path(__file__).parent.absolute() / "config.test.toml", "rb") as config_file:
        config = load_toml(config_file)

elif ENV_TYPE == "dev":
    logger.info("Loading development config")
    with open(pathlib.Path(__file__).parent.absolute() / "config.dev.toml", "rb") as config_file:
        config = load_toml(config_file)

else:
    logger.info("No ENV_TYPE set, loading production config")
    with open(pathlib.Path(__file__).parent.absolute() / "config.prod.toml", "rb") as config_file:
        config = load_toml(config_file)


class Settings(BaseSettings):
    # Environment variables
    rapid_api_secret_key: str = getenv("RAPID_API_SECRET_KEY", "")
    geonames_username: str = getenv("GEONAMES_USERNAME", "")
    env_type: str | bool = ENV_TYPE

    # Config file
    admin_email: str = config["admin_email"]
    allowed_hosts: list = config["allowed_hosts"]
    allowed_cors_origins: list = config["allowed_cors_origins"]
    debug: bool = config["debug"]
    docs_url: str | None = config["docs_url"]
    redoc_url: str | None = config["redoc_url"]
    secret_key_name: str = config["secret_key_name"]

    # Common settings
    log_level: int = int(config["log_level"])
    LOGGING_CONFIG: dict = {
        "version": 1,
        "disable_existing_loggers": False,
        "formatters": {
            "default": {
                "()": "uvicorn.logging.DefaultFormatter",
                "fmt": "[%(asctime)s] %(levelprefix)s %(message)s - Module: %(name)s",
                "use_colors": None,
                "datefmt": "%Y-%m-%d %H:%M:%S",
            },
            "access": {
                "()": "uvicorn.logging.AccessFormatter",
                "fmt": "[%(asctime)s] %(levelprefix)s %(message)s - Module: %(name)s",
                "datefmt": "%Y-%m-%d %H:%M:%S",
            },
        },
        "handlers": {
            "default": {
                "formatter": "default",
                "class": "logging.StreamHandler",
                "stream": "ext://sys.stderr",
            },
            "access": {
                "formatter": "access",
                "class": "logging.StreamHandler",
                "stream": "ext://sys.stdout",
            },
        },
        "loggers": {
            "uvicorn": {"handlers": ["default"], "level": log_level, "propagate": False},
            "uvicorn.error": {
                "level": log_level,
            },
            "root": {
                "handlers": ["default"],
                "level": log_level,
            },
            "uvicorn.access": {"handlers": ["access"], "level": log_level, "propagate": False},
        },
    }


settings = Settings()
````

## File: app/middleware/__init__.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""
````

## File: app/middleware/secret_key_checker_middleware.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""

from starlette.datastructures import Headers
from starlette.responses import JSONResponse
from starlette.types import ASGIApp, Receive, Scope, Send
import logging

class SecretKeyCheckerMiddleware:
    def __init__(self, app: ASGIApp, secret_key_name: str, secret_keys: list = []) -> None:
        self.app = app
        self.secret_key_values = list(secret_keys)
        self.secret_key_name = secret_key_name

        if not self.secret_key_name or not self.secret_key_values:
            logging.critical("Secret key name or secret key values not set. The middleware will let all requests pass through!")

    async def __call__(self, scope: Scope, receive: Receive, send: Send) -> None:
        headers = Headers(scope=scope)
        header_key = headers.get(self.secret_key_name, "").split(":")[0]
        is_valid_key = False

        for key in self.secret_key_values:
            if header_key == key:
                is_valid_key = True
                break

        if is_valid_key:
            await self.app(scope, receive, send)

        else:
            response = JSONResponse(status_code=400, content={"status": "KO", "message": "Bad request"})

            await response(scope, receive, send)
````

## File: app/routers/__init__.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""
````

## File: app/routers/main_router.py
````python
# External Libraries
from fastapi import APIRouter, Request
from fastapi.responses import JSONResponse
from logging import getLogger
from kerykeion import (
    AstrologicalSubject, 
    KerykeionChartSVG, 
    SynastryAspects, 
    NatalAspects, 
    RelationshipScoreFactory, 
    CompositeSubjectFactory
)
from kerykeion.settings.config_constants import DEFAULT_ACTIVE_POINTS, DEFAULT_ACTIVE_ASPECTS

# Local
from ..utils.internal_server_error_json_response import InternalServerErrorJsonResponse
from ..utils.get_time_from_google import get_time_from_google
from ..utils.write_request_to_log import get_write_request_to_log
from ..types.request_models import (
    BirthDataRequestModel,
    BirthChartRequestModel,
    SynastryChartRequestModel,
    TransitChartRequestModel,
    RelationshipScoreRequestModel,
    SynastryAspectsRequestModel,
    NatalAspectsRequestModel,
    CompositeChartRequestModel
)
from ..types.response_models import (
    BirthDataResponseModel,
    BirthChartResponseModel,
    SynastryChartResponseModel,
    RelationshipScoreResponseModel,
    SynastryAspectsResponseModel,
    CompositeChartResponseModel,
    CompositeAspectsResponseModel,
    TransitAspectsResponseModel,
    TransitChartResponseModel
)

logger = getLogger(__name__)
write_request_to_log = get_write_request_to_log(logger)

router = APIRouter()

GEONAMES_ERROR_MESSAGE = "City/Nation name error or invalid GeoNames username. Please check your username or city name and try again. You can create a free username here: https://www.geonames.org/login/. If you want to bypass the usage of GeoNames, please remove the geonames_username field from the request. Note: The nation field should be the country code (e.g. US, UK, FR, DE, etc.)."

@router.get("/api/v4/health", response_description="Health check", include_in_schema=False)
async def health(request: Request) -> JSONResponse:
    """
    Health check endpoint.
    """

    write_request_to_log(20, request, "Health check")

    return JSONResponse(content={"status": "OK"}, status_code=200)


@router.get("/", response_description="Status of the API", response_model=BirthDataResponseModel, include_in_schema=False)
async def status(request: Request) -> JSONResponse:
    """
    Returns the status of the API.
    """

    from ..config.settings import settings

    write_request_to_log(20, request, "API is up and running")
    response_dict = {
        "status": "OK",
        "environment": settings.env_type,
        "debug": settings.debug,
    }

    return JSONResponse(content=response_dict, status_code=200)


@router.get("/api/v4/now", response_description="Current astrological data", response_model=BirthDataResponseModel)
async def get_now(request: Request) -> JSONResponse:
    """
    Retrieve astrological data for the current moment.
    """

    # Get current UTC time from the time API
    write_request_to_log(20, request, "Getting current astrological data")
    
    logger.debug("Getting current UTC time from the time API")
    try:
        utc_datetime = get_time_from_google()
        datetime_dict = {
            "year": utc_datetime.year, # type: ignore
            "month": utc_datetime.month, # type: ignore
            "day": utc_datetime.day, # type: ignore
            "hour": utc_datetime.hour, # type: ignore
            "minute": utc_datetime.minute, # type: ignore
            "second": utc_datetime.second, # type: ignore
        }
    except Exception as e:
        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse
    logger.debug(f"Current UTC time: {datetime_dict}")

    try:
        # On some Cloud providers, the time is not set correctly, so we need to get the current UTC time from the time API
        today_subject = AstrologicalSubject(
            city="GMT",
            nation="UK",
            lat=51.477928,
            lng=-0.001545,
            tz_str="GMT",
            year=datetime_dict["year"],
            month=datetime_dict["month"],
            day=datetime_dict["day"],
            hour=datetime_dict["hour"],
            minute=datetime_dict["minute"],
            online=False,
        )

        response_dict = {"status": "OK", "data": today_subject.model().model_dump()}

        return JSONResponse(content=response_dict, status_code=200)

    except Exception as e:
        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/birth-data", response_description="Birth data", response_model=BirthDataResponseModel)
async def birth_data(birth_data_request: BirthDataRequestModel, request: Request):
    """
    Retrieve astrological data for a specific birth date. Does not include the chart nor the aspects.
    """

    write_request_to_log(20, request, f"Birth data request")

    subject = birth_data_request.subject

    try:
        astrological_subject = AstrologicalSubject(
            name=subject.name,
            year=subject.year,
            month=subject.month,
            day=subject.day,
            hour=subject.hour,
            minute=subject.minute,
            city=subject.city,
            nation=subject.nation,
            lat=subject.latitude,
            lng=subject.longitude,
            tz_str=subject.timezone,
            zodiac_type=subject.zodiac_type, # type: ignore
            sidereal_mode=subject.sidereal_mode,
            houses_system_identifier=subject.houses_system_identifier, # type: ignore
            perspective_type=subject.perspective_type, # type: ignore
            geonames_username=subject.geonames_username,
            online=True if subject.geonames_username else False,
        )

        data = astrological_subject.model().model_dump()

        response_dict = {"status": "OK", "data": data}

        return JSONResponse(content=response_dict, status_code=200)

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/birth-chart", response_description="Birth chart", response_model=BirthChartResponseModel)
async def birth_chart(request_body: BirthChartRequestModel, request: Request):
    """
    Retrieve an astrological birth chart for a specific birth date. Includes the data for the subject and the aspects.
    """

    write_request_to_log(20, request, f"Birth chart request")

    subject = request_body.subject

    try:
        astrological_subject = AstrologicalSubject(
            name=subject.name,
            year=subject.year,
            month=subject.month,
            day=subject.day,
            hour=subject.hour,
            minute=subject.minute,
            city=subject.city,
            nation=subject.nation,
            lat=subject.latitude,
            lng=subject.longitude,
            tz_str=subject.timezone,
            zodiac_type=subject.zodiac_type, # type: ignore
            sidereal_mode=subject.sidereal_mode,
            houses_system_identifier=subject.houses_system_identifier, # type: ignore
            perspective_type=subject.perspective_type, # type: ignore
            geonames_username=subject.geonames_username,
            online=True if subject.geonames_username else False,
        )

        data = astrological_subject.model().model_dump()

        kerykeion_chart = KerykeionChartSVG(
            astrological_subject,
            theme=request_body.theme,
            chart_language=request_body.language or "EN",
            active_points=request_body.active_points or DEFAULT_ACTIVE_POINTS,
            active_aspects=request_body.active_aspects or DEFAULT_ACTIVE_ASPECTS,
        )

        if request_body.wheel_only:
            svg = kerykeion_chart.makeWheelOnlyTemplate(minify=True)
        else:
            svg = kerykeion_chart.makeTemplate(minify=True)

        return JSONResponse(
            content={
                "status": "OK",
                "chart": svg,
                "data": data,
                "aspects": [aspect.model_dump() for aspect in kerykeion_chart.aspects_list]
            },
            status_code=200,
        )

    except Exception as e:
        # If error contains "wrong username"
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/synastry-chart", response_description="Synastry data", response_model=SynastryChartResponseModel)
async def synastry_chart(synastry_chart_request: SynastryChartRequestModel, request: Request):
    """
    Retrieve a synastry chart between two subjects. Includes the data for the subjects and the aspects.
    """

    write_request_to_log(20, request, f"Synastry chart request")

    first_subject = synastry_chart_request.first_subject
    second_subject = synastry_chart_request.second_subject

    try:
        first_astrological_subject = AstrologicalSubject(
            name=first_subject.name,
            year=first_subject.year,
            month=first_subject.month,
            day=first_subject.day,
            hour=first_subject.hour,
            minute=first_subject.minute,
            city=first_subject.city,
            nation=first_subject.nation,
            lat=first_subject.latitude,
            lng=first_subject.longitude,
            tz_str=first_subject.timezone,
            zodiac_type=first_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=first_subject.geonames_username,
            online=True if first_subject.geonames_username else False,
        )

        second_astrological_subject = AstrologicalSubject(
            name=second_subject.name,
            year=second_subject.year,
            month=second_subject.month,
            day=second_subject.day,
            hour=second_subject.hour,
            minute=second_subject.minute,
            city=second_subject.city,
            nation=second_subject.nation,
            lat=second_subject.latitude,
            lng=second_subject.longitude,
            tz_str=second_subject.timezone,
            zodiac_type=second_subject.zodiac_type, # type: ignore
            sidereal_mode=second_subject.sidereal_mode,
            houses_system_identifier=second_subject.houses_system_identifier, # type: ignore
            perspective_type=second_subject.perspective_type, # type: ignore
            geonames_username=second_subject.geonames_username,
            online=True if second_subject.geonames_username else False,
        )

        kerykeion_chart = KerykeionChartSVG(
            first_astrological_subject,
            second_obj=second_astrological_subject,
            chart_type="Synastry",
            theme=synastry_chart_request.theme,
            chart_language=synastry_chart_request.language or "EN",
            active_points=synastry_chart_request.active_points or DEFAULT_ACTIVE_POINTS,
            active_aspects=synastry_chart_request.active_aspects or DEFAULT_ACTIVE_ASPECTS,
        )

        if synastry_chart_request.wheel_only:
            svg = kerykeion_chart.makeWheelOnlyTemplate(minify=True)
        else:
            svg = kerykeion_chart.makeTemplate(minify=True)

        return JSONResponse(
            content={
                "status": "OK",
                "chart": svg,
                "aspects": [aspect.model_dump() for aspect in kerykeion_chart.aspects_list],
                "data": {
                    "first_subject": first_astrological_subject.model().model_dump(),
                    "second_subject": second_astrological_subject.model().model_dump(),
                },
            },
            status_code=200,
        )

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/transit-chart", response_description="Transit data", response_model=TransitChartResponseModel)
async def transit_chart(transit_chart_request: TransitChartRequestModel, request: Request):
    """
    Retrieve a transit chart for a specific subject. Includes the data for the subject and the aspects.
    """

    write_request_to_log(20, request, f"Transit chart request")

    first_subject = transit_chart_request.first_subject
    second_subject = transit_chart_request.transit_subject

    try:
        first_astrological_subject = AstrologicalSubject(
            name=first_subject.name,
            year=first_subject.year,
            month=first_subject.month,
            day=first_subject.day,
            hour=first_subject.hour,
            minute=first_subject.minute,
            city=first_subject.city,
            nation=first_subject.nation,
            lat=first_subject.latitude,
            lng=first_subject.longitude,
            tz_str=first_subject.timezone,
            zodiac_type=first_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=first_subject.geonames_username,
            online=True if first_subject.geonames_username else False,
        )

        second_astrological_subject = AstrologicalSubject(
            name="Transit",
            year=second_subject.year,
            month=second_subject.month,
            day=second_subject.day,
            hour=second_subject.hour,
            minute=second_subject.minute,
            city=second_subject.city,
            nation=second_subject.nation,
            lat=second_subject.latitude,
            lng=second_subject.longitude,
            tz_str=second_subject.timezone,
            zodiac_type=first_astrological_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=second_subject.geonames_username,
            online=True if second_subject.geonames_username else False,
        )

        kerykeion_chart = KerykeionChartSVG(
            first_astrological_subject,
            second_obj=second_astrological_subject,
            chart_type="Transit",
            theme=transit_chart_request.theme,
            chart_language=transit_chart_request.language or "EN",
            active_points=transit_chart_request.active_points or DEFAULT_ACTIVE_POINTS,
            active_aspects=transit_chart_request.active_aspects or DEFAULT_ACTIVE_ASPECTS,
        )

        if transit_chart_request.wheel_only:
            svg = kerykeion_chart.makeWheelOnlyTemplate(minify=True)
        else:
            svg = kerykeion_chart.makeTemplate(minify=True)

        return JSONResponse(
            content={
                "status": "OK",
                "chart": svg,
                "aspects": [aspect.model_dump() for aspect in kerykeion_chart.aspects_list],
                "data": {
                    "subject": first_astrological_subject.model().model_dump(),
                    "transit": second_astrological_subject.model().model_dump(),
                },
            },
            status_code=200,
        )

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/transit-aspects-data", response_description="Transit aspects data", response_model=TransitAspectsResponseModel)
async def transit_aspects_data(transit_chart_request: TransitChartRequestModel, request: Request) -> JSONResponse:
    """
    Retrieve transit aspects and data for a specific subject. Does not include the chart.
    """

    write_request_to_log(20, request, f"Transit aspects data request")

    first_subject = transit_chart_request.first_subject
    second_subject = transit_chart_request.transit_subject

    try:
        first_astrological_subject = AstrologicalSubject(
            name=first_subject.name,
            year=first_subject.year,
            month=first_subject.month,
            day=first_subject.day,
            hour=first_subject.hour,
            minute=first_subject.minute,
            city=first_subject.city,
            nation=first_subject.nation,
            lat=first_subject.latitude,
            lng=first_subject.longitude,
            tz_str=first_subject.timezone,
            zodiac_type=first_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=first_subject.geonames_username,
            online=True if first_subject.geonames_username else False,
        )

        second_astrological_subject = AstrologicalSubject(
            name="Transit",
            year=second_subject.year,
            month=second_subject.month,
            day=second_subject.day,
            hour=second_subject.hour,
            minute=second_subject.minute,
            city=second_subject.city,
            nation=second_subject.nation,
            lat=second_subject.latitude,
            lng=second_subject.longitude,
            tz_str=second_subject.timezone,
            zodiac_type=first_astrological_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=second_subject.geonames_username,
            online=True if second_subject.geonames_username else False,
        )

        aspects = SynastryAspects(
            first_astrological_subject,
            second_astrological_subject,
            active_points=transit_chart_request.active_points or DEFAULT_ACTIVE_POINTS,
            active_aspects=transit_chart_request.active_aspects or DEFAULT_ACTIVE_ASPECTS,
        ).relevant_aspects

        return JSONResponse(
            content={
                "status": "OK",
                "data": {
                    "subject": first_astrological_subject.model().model_dump(),
                    "transit": second_astrological_subject.model().model_dump(),
                },
                "aspects": [aspect.model_dump() for aspect in aspects],
            },
            status_code=200,
        )

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/synastry-aspects-data", response_description="Synastry aspects data", response_model=SynastryAspectsResponseModel)
async def synastry_aspects_data(aspects_request_content: SynastryAspectsRequestModel, request: Request) -> JSONResponse:
    """
    Retrieve synastry aspects between two subjects. Does not include the chart.
    """

    write_request_to_log(20, request, f"Synastry aspects data request")

    first_subject = aspects_request_content.first_subject
    second_subject = aspects_request_content.second_subject

    try:
        first_astrological_subject = AstrologicalSubject(
            name=first_subject.name,
            year=first_subject.year,
            month=first_subject.month,
            day=first_subject.day,
            hour=first_subject.hour,
            minute=first_subject.minute,
            city=first_subject.city,
            nation=first_subject.nation,
            lat=first_subject.latitude,
            lng=first_subject.longitude,
            tz_str=first_subject.timezone,
            zodiac_type=first_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=first_subject.geonames_username,
            online=True if first_subject.geonames_username else False,
        )

        second_astrological_subject = AstrologicalSubject(
            name=second_subject.name,
            year=second_subject.year,
            month=second_subject.month,
            day=second_subject.day,
            hour=second_subject.hour,
            minute=second_subject.minute,
            city=second_subject.city,
            nation=second_subject.nation,
            lat=second_subject.latitude,
            lng=second_subject.longitude,
            tz_str=second_subject.timezone,
            zodiac_type=second_subject.zodiac_type, # type: ignore
            sidereal_mode=second_subject.sidereal_mode,
            houses_system_identifier=second_subject.houses_system_identifier, # type: ignore
            perspective_type=second_subject.perspective_type, # type: ignore
            geonames_username=second_subject.geonames_username,
            online=True if second_subject.geonames_username else False,
        )

        aspects = SynastryAspects(
            first_astrological_subject,
            second_astrological_subject,
            active_points=aspects_request_content.active_points or DEFAULT_ACTIVE_POINTS,
            active_aspects=aspects_request_content.active_aspects or DEFAULT_ACTIVE_ASPECTS,
        ).relevant_aspects

        return JSONResponse(
            content={
                "status": "OK",
                "data": {
                    "first_subject": first_astrological_subject.model().model_dump(),
                    "second_subject": second_astrological_subject.model().model_dump(),
                },
                "aspects": [aspect.model_dump() for aspect in aspects],
            },
            status_code=200,
        )

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/natal-aspects-data", response_description="Birth aspects data", response_model=SynastryAspectsResponseModel)
async def natal_aspects_data(aspects_request_content: NatalAspectsRequestModel, request: Request) -> JSONResponse:
    """
    Retrieve natal aspects and data for a specific subject. Does not include the chart.
    """

    write_request_to_log(20, request, f"Natal aspects data request")

    subject = aspects_request_content.subject

    try:
        first_astrological_subject = AstrologicalSubject(
            name=subject.name,
            year=subject.year,
            month=subject.month,
            day=subject.day,
            hour=subject.hour,
            minute=subject.minute,
            city=subject.city,
            nation=subject.nation,
            lat=subject.latitude,
            lng=subject.longitude,
            tz_str=subject.timezone,
            zodiac_type=subject.zodiac_type, # type: ignore
            sidereal_mode=subject.sidereal_mode,
            houses_system_identifier=subject.houses_system_identifier, # type: ignore
            perspective_type=subject.perspective_type, # type: ignore
            geonames_username=subject.geonames_username,
            online=True if subject.geonames_username else False,
        )

        aspects = NatalAspects(
            first_astrological_subject,
            active_points=aspects_request_content.active_points or DEFAULT_ACTIVE_POINTS,
            active_aspects=aspects_request_content.active_aspects or DEFAULT_ACTIVE_ASPECTS,
        ).relevant_aspects

        return JSONResponse(
            content={
                "status": "OK",
                "data": {"subject": first_astrological_subject.model().model_dump()},
                "aspects": [aspect.model_dump() for aspect in aspects],
            },
            status_code=200,
        )

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/relationship-score", response_description="Relationship score", response_model=RelationshipScoreResponseModel)
async def relationship_score(relationship_score_request: RelationshipScoreRequestModel, request: Request) -> JSONResponse:
    """
    Calculates the relevance of the relationship between two subjects using the Ciro Discepolo method.

    Results:
        - 0 to 5: Minimal relationship
        - 5 to 10: Medium relationship
        - 10 to 15: Important relationship
        - 15 to 20: Very important relationship
        - 20 to 35: Exceptional relationship
        - 30 and above: Rare Exceptional relationship

    More details: https://www-cirodiscepolo-it.translate.goog/Articoli/Discepoloele.htm?_x_tr_sl=it&_x_tr_tl=en&_x_tr_hl=it&_x_tr_pto=wapp
    """

    first_subject = relationship_score_request.first_subject
    second_subject = relationship_score_request.second_subject

    write_request_to_log(20, request, f"Getting composite data for: {first_subject} and {second_subject}")

    try:
        first_astrological_subject = AstrologicalSubject(
            name=first_subject.name,
            year=first_subject.year,
            month=first_subject.month,
            day=first_subject.day,
            hour=first_subject.hour,
            minute=first_subject.minute,
            city=first_subject.city,
            nation=first_subject.nation,
            lat=first_subject.latitude,
            lng=first_subject.longitude,
            tz_str=first_subject.timezone,
            zodiac_type=first_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=first_subject.geonames_username,
            online=True if first_subject.geonames_username else False,
        )

        second_astrological_subject = AstrologicalSubject(
            name=second_subject.name,
            year=second_subject.year,
            month=second_subject.month,
            day=second_subject.day,
            hour=second_subject.hour,
            minute=second_subject.minute,
            city=second_subject.city,
            nation=second_subject.nation,
            lat=second_subject.latitude,
            lng=second_subject.longitude,
            tz_str=second_subject.timezone,
            zodiac_type=second_subject.zodiac_type, # type: ignore
            sidereal_mode=second_subject.sidereal_mode,
            houses_system_identifier=second_subject.houses_system_identifier, # type: ignore
            perspective_type=second_subject.perspective_type, # type: ignore
            geonames_username=second_subject.geonames_username,
            online=True if second_subject.geonames_username else False,
        )

        score_factory = RelationshipScoreFactory(first_astrological_subject, second_astrological_subject)
        score_model = score_factory.get_relationship_score()

        response_content = {
            "status": "OK",
            "score": score_model.score_value,
            "score_description": score_model.score_description,
            "is_destiny_sign": score_model.is_destiny_sign,
            "aspects": [aspect.model_dump() for aspect in score_model.aspects],
            "data": {
                "first_subject": first_astrological_subject.model().model_dump(),
                "second_subject": second_astrological_subject.model().model_dump(),
            },
        }

        return JSONResponse(content=response_content, status_code=200)

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/composite-chart", response_description="Composite data", response_model=CompositeChartResponseModel)
async def composite_chart(composite_chart_request: CompositeChartRequestModel, request: Request) -> JSONResponse:
    """
    Retrieve a composite chart between two subjects. Includes the data for the subjects and the aspects.
    The method used is the midpoint method.
    """

    first_subject = composite_chart_request.first_subject
    second_subject = composite_chart_request.second_subject

    write_request_to_log(20, request, f"Getting composite data for: {first_subject} and {second_subject}")

    try:
        first_astrological_subject = AstrologicalSubject(
            name=first_subject.name,
            year=first_subject.year,
            month=first_subject.month,
            day=first_subject.day,
            hour=first_subject.hour,
            minute=first_subject.minute,
            city=first_subject.city,
            nation=first_subject.nation,
            lat=first_subject.latitude,
            lng=first_subject.longitude,
            tz_str=first_subject.timezone,
            zodiac_type=first_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=first_subject.geonames_username,
            online=True if first_subject.geonames_username else False,
        )

        second_astrological_subject = AstrologicalSubject(
            name=second_subject.name,
            year=second_subject.year,
            month=second_subject.month,
            day=second_subject.day,
            hour=second_subject.hour,
            minute=second_subject.minute,
            city=second_subject.city,
            nation=second_subject.nation,
            lat=second_subject.latitude,
            lng=second_subject.longitude,
            tz_str=second_subject.timezone,
            zodiac_type=second_subject.zodiac_type, # type: ignore
            sidereal_mode=second_subject.sidereal_mode,
            houses_system_identifier=second_subject.houses_system_identifier, # type: ignore
            perspective_type=second_subject.perspective_type, # type: ignore
            geonames_username=second_subject.geonames_username,
            online=True if second_subject.geonames_username else False,
        )

        composite_factory = CompositeSubjectFactory(first_astrological_subject, second_astrological_subject)
        composite_subject = composite_factory.get_midpoint_composite_subject_model()

        kerykeion_chart = KerykeionChartSVG(
            composite_subject,
            chart_type="Composite",
            theme=composite_chart_request.theme
        )

        if composite_chart_request.wheel_only:
            svg = kerykeion_chart.makeWheelOnlyTemplate(minify=True)
        else:
            svg = kerykeion_chart.makeTemplate(minify=True)

        composite_subject_dict = composite_subject.model_dump()
        for key in ["first_subject", "second_subject"]:
            if key in composite_subject_dict:
                composite_subject_dict.pop(key)

        return JSONResponse(
            content={
                "status": "OK",
                "chart": svg,
                "aspects": [aspect.model_dump() for aspect in kerykeion_chart.aspects_list],
                "data": {
                    "composite_subject": composite_subject_dict,
                    "first_subject": first_astrological_subject.model().model_dump(),
                    "second_subject": second_astrological_subject.model().model_dump(),
                },
            },
            status_code=200,
        )

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse


@router.post("/api/v4/composite-aspects-data", response_description="Composite aspects data", response_model=CompositeAspectsResponseModel)
async def composite_aspects_data(composite_chart_request: CompositeChartRequestModel, request: Request) -> JSONResponse:
    """
    Retrieves the data and the aspects for a composite chart between two subjects. Does not include the chart.
    """

    first_subject = composite_chart_request.first_subject
    second_subject = composite_chart_request.second_subject

    write_request_to_log(20, request, f"Getting composite data for: {first_subject} and {second_subject}")

    try:
        first_astrological_subject = AstrologicalSubject(
            name=first_subject.name,
            year=first_subject.year,
            month=first_subject.month,
            day=first_subject.day,
            hour=first_subject.hour,
            minute=first_subject.minute,
            city=first_subject.city,
            nation=first_subject.nation,
            lat=first_subject.latitude,
            lng=first_subject.longitude,
            tz_str=first_subject.timezone,
            zodiac_type=first_subject.zodiac_type, # type: ignore
            sidereal_mode=first_subject.sidereal_mode,
            houses_system_identifier=first_subject.houses_system_identifier, # type: ignore
            perspective_type=first_subject.perspective_type, # type: ignore
            geonames_username=first_subject.geonames_username,
            online=True if first_subject.geonames_username else False,
        )

        second_astrological_subject = AstrologicalSubject(
            name=second_subject.name,
            year=second_subject.year,
            month=second_subject.month,
            day=second_subject.day,
            hour=second_subject.hour,
            minute=second_subject.minute,
            city=second_subject.city,
            nation=second_subject.nation,
            lat=second_subject.latitude,
            lng=second_subject.longitude,
            tz_str=second_subject.timezone,
            zodiac_type=second_subject.zodiac_type, # type: ignore
            sidereal_mode=second_subject.sidereal_mode,
            houses_system_identifier=second_subject.houses_system_identifier, # type: ignore
            perspective_type=second_subject.perspective_type, # type: ignore
            geonames_username=second_subject.geonames_username,
            online=True if second_subject.geonames_username else False,
        )

        composite_factory = CompositeSubjectFactory(first_astrological_subject, second_astrological_subject)
        composite_data = composite_factory.get_midpoint_composite_subject_model()
        aspects = NatalAspects(
            composite_data,
            active_points=composite_chart_request.active_points or DEFAULT_ACTIVE_POINTS,
            active_aspects=composite_chart_request.active_aspects or DEFAULT_ACTIVE_ASPECTS,
        ).relevant_aspects

        composite_subject_dict = composite_data.model_dump()
        for key in ["first_subject", "second_subject"]:
            if key in composite_subject_dict:
                composite_subject_dict.pop(key)

        return JSONResponse(
            content={
                "status": "OK",
                "data": {
                    "composite_subject": composite_subject_dict,
                    "first_subject": first_astrological_subject.model().model_dump(),
                    "second_subject": second_astrological_subject.model().model_dump(),
                },
                "aspects": [aspect.model_dump() for aspect in aspects],
            },
            status_code=200,
        )

    except Exception as e:
        if "data found for this city" in str(e):
            write_request_to_log(40, request, e)
            return JSONResponse(
                content={
                    "status": "ERROR",
                    "message": GEONAMES_ERROR_MESSAGE,
                },
                status_code=400,
            )

        write_request_to_log(40, request, e)
        return InternalServerErrorJsonResponse
````

## File: app/tmp/README.md
````markdown
This directory is used to store temporary files.
````

## File: app/types/request_models.py
````python
from pydantic import BaseModel, Field, field_validator, model_validator
from typing import Optional, get_args, Union
from kerykeion.kr_types.kr_models import ActiveAspect
from pytz import all_timezones
from kerykeion.kr_types.kr_literals import KerykeionChartTheme, KerykeionChartLanguage, SiderealMode, ZodiacType, HousesSystemIdentifier, PerspectiveType, AxialCusps, Planet
from kerykeion.settings.config_constants import DEFAULT_ACTIVE_POINTS, DEFAULT_ACTIVE_ASPECTS
from abc import ABC

class AbstractBaseSubjectModel(BaseModel, ABC):
    year: int = Field(description="The year of birth.", examples=[1980])
    month: int = Field(description="The month of birth.", examples=[12])
    day: int = Field(description="The day of birth.", examples=[12])
    hour: int = Field(description="The hour of birth.", examples=[12])
    minute: int = Field(description="The minute of birth.", examples=[12])
    longitude: Optional[float] = Field(description="The longitude of the birth location. Defaults on London.", examples=[0], default=None)
    latitude: Optional[float] = Field(description="The latitude of the birth location. Defaults on London.", examples=[51.4825766], default=None)
    city: str = Field(description="The name of city of birth.", examples=["London"])
    nation: Optional[str] = Field(default="null", description="The name of the nation of birth.", examples=["GB"])
    timezone: Optional[str] = Field(description="The timezone of the birth location.", examples=["Europe/London"], default=None)
    geonames_username: Optional[str] = Field(description="The username for the Geonames API.", examples=[None], default=None)


    @field_validator("longitude")
    def validate_longitude(cls, value):
        if value is None:
            return None
        if value < -180 or value > 180:
            raise ValueError(f"Invalid longitude '{value}'. Please use a value between -180 and 180.")
        return value

    @field_validator("latitude")
    def validate_latitude(cls, value):
        if value is None:
            return None
        if value < -90 or value > 90:
            raise ValueError(f"Invalid latitude '{value}'. Please use a value between -90 and 90.")
        return value

    @field_validator("timezone")
    def validate_timezone(cls, value):
        if value is None:
            return None
        if value not in all_timezones:
            raise ValueError(f"Invalid timezone '{value}'. Please use a valid timezone. You can find a list of valid timezones at https://en.wikipedia.org/wiki/List_of_tz_database_time_zones.")
        return value

    @field_validator("month")
    def validate_month(cls, value):
        if value is None:
            return None
        if value < 1 or value > 12:
            raise ValueError(f"Invalid month '{value}'. Please use a value between 1 and 12.")
        return value

    @field_validator("day")
    def validate_day(cls, value, values):
        month = values.data.get("month")

        if month in [1, 3, 5, 7, 8, 10, 12]:
            if value < 1 or value > 31:
                raise ValueError(f"Invalid day '{value}'. Please use a value between 1 and 31.")
        elif month in [4, 6, 9, 11]:
            if value < 1 or value > 30:
                raise ValueError(f"Invalid day '{value}'. Please use a value between 1 and 30.")
        elif month == 2:
            if value < 1 or value > 29:
                raise ValueError(f"Invalid day '{value}'. Please use a value between 1 and 29.")
        return value

    @field_validator("hour")
    def validate_hour(cls, value):
        if value is None:
            return None
        if value < 0 or value > 23:
            raise ValueError(f"Invalid hour '{value}'. Please use a value between 0 and 23.")
        return value

    @field_validator("minute")
    def validate_minute(cls, value):
        if value is None:
            return None
        if value < 0 or value > 59:
            raise ValueError(f"Invalid minute '{value}'. Please use a value between 0 and 59.")
        return value

    @field_validator("year")
    def validate_year(cls, value):
        if value is None:
            return None
        if value < 1800 or value > 2100:
            raise ValueError(f"Invalid year '{value}'. Please use a value between 1800 and 2300.")
        return value

    @field_validator("nation")
    def validate_nation(cls, value):
        if not value:
            return "null"

        if len(value) != 2 or not value.isalpha():  # Esattamente 2 lettere
            raise ValueError(
                f"Invalid nation code: '{value}'. It must be a 2-letter country code following the ISO 3166-1 alpha-2 standard."
            )

        return value

    @model_validator(mode="after")
    def check_lat_lng_tz_or_geonames(self):
        lat = self.latitude
        lng = self.longitude
        tz = self.timezone
        geonames = self.geonames_username

        # If latitude, longitude, and timezone are all missing, geonames_username must be provided
        if lat is None and lng is None and tz is None:
            if not geonames:
                raise ValueError("Either provide latitude, longitude, timezone or specify geonames_username.")

        # If any one of latitude, longitude, or timezone is missing (but not all), either fill them all or use geonames_username
        missing_fields = sum(1 for f in [lat, lng, tz] if f is None)
        if 0 < missing_fields < 3 and not geonames:
            raise ValueError("Please provide all missing fields (latitude, longitude, timezone) or specify geonames_username.")

        if geonames and (lat or lng or tz):
            self.latitude = None
            self.longitude = None
            self.timezone = None

        return self

class SubjectModel(AbstractBaseSubjectModel):
    """
    The request model for the Birth Chart endpoint.
    """

    name: str = Field(description="The name of the person to get the Birth Chart for.", examples=["John Doe"])
    zodiac_type: Optional[ZodiacType] = Field(default="Tropic", description="The type of zodiac used (Tropic or Sidereal).", examples=list(get_args(ZodiacType)))
    sidereal_mode: Union[SiderealMode, None] = Field(default=None, description="The sidereal mode used.", examples=[None])
    perspective_type: Union[PerspectiveType, None] = Field(default="Apparent Geocentric", description="The perspective type used.", examples=list(get_args(PerspectiveType)))
    houses_system_identifier: Union[HousesSystemIdentifier, None] = Field(
        default="P",
        examples=['P'],
        description=(
            "The house system to use. The following are the available house systems: "
            "A = equal "
            "B = Alcabitius "
            "C = Campanus "
            "D = equal (MC) "
            "F = Carter poli-equ. "
            "H = horizon/azimut "
            "I = Sunshine "
            "i = Sunshine/alt. "
            "K = Koch "
            "L = Pullen SD "
            "M = Morinus "
            "N = equal/1=Aries "
            "O = Porphyry "
            "P = Placidus "
            "Q = Pullen SR "
            "R = Regiomontanus "
            "S = Sripati "
            "T = Polich/Page "
            "U = Krusinski-Pisa-Goelzer "
            "V = equal/Vehlow "
            "W = equal/whole sign "
            "X = axial rotation system/Meridian houses "
            "Y = APC houses "
            "Usually the standard is Placidus (P)"
        )
    )

    @field_validator("zodiac_type")
    def validate_zodiac_type(cls, value, info):
        if info.data.get('sidereal_mode') and value != "Sidereal":
            raise ValueError(f"Invalid zodiac_type '{value}'. Please use 'Sidereal' when sidereal_mode is set.")
        return value

    @field_validator("sidereal_mode")
    def validate_sidereal_mode(cls, value, info):
        # If sidereal mode is set, zodiac type must be Sidereal
        if value and info.data.get('zodiac_type') != "Sidereal":
            raise ValueError(f"Invalid sidereal_mode '{value}'. Please use 'Sidereal' as zodiac_type when sidereal_mode is set. If you want to use the default sidereal mode, do not set this field or set it to None.")
        return value

    @field_validator("perspective_type")
    def validate_perspective_type(cls, value, info):
        # If it's none, set it to the default value
        if not value:
            return "Apparent Geocentric"
        return value

    @field_validator("houses_system_identifier")
    def validate_houses_system_identifier(cls, value, info):
        # If it's none, set it to the default value
        if not value:
            return "P"
        return value

class TransitSubjectModel(AbstractBaseSubjectModel):
    ...

class BirthChartRequestModel(BaseModel):
    """
    The request model for the Birth Chart endpoint.
    """

    subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    theme: Optional[KerykeionChartTheme] = Field(default="classic", description="The theme of the chart.", examples=["classic", "light", "dark", "dark-high-contrast"])
    language: Optional[KerykeionChartLanguage] = Field(default="EN", description="The language of the chart.", examples=list(get_args(KerykeionChartLanguage)))
    wheel_only: Optional[bool] = Field(default=False, description="If set to True, only the zodiac wheel will be returned. No additional information will be displayed.")
    active_points: Optional[list[Union[Planet, AxialCusps]]] = Field(default=DEFAULT_ACTIVE_POINTS, description="The active points to display in the chart.", examples=[DEFAULT_ACTIVE_POINTS])
    active_aspects: Optional[list[ActiveAspect]] = Field(default=DEFAULT_ACTIVE_ASPECTS, description="The active aspects to display in the chart.", examples=[DEFAULT_ACTIVE_ASPECTS])

class SynastryChartRequestModel(BaseModel):
    """
    The request model for the Synastry Chart endpoint.
    """

    first_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    second_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    theme: Optional[KerykeionChartTheme] = Field(default="classic", description="The theme of the chart.", examples=["classic", "light", "dark", "dark-high-contrast"])
    language: Optional[KerykeionChartLanguage] = Field(default="EN", description="The language of the chart.", examples=list(get_args(KerykeionChartLanguage)))
    wheel_only: Optional[bool] = Field(default=False, description="If set to True, only the zodiac wheel will be returned. No additional information will be displayed.")
    active_points: Optional[list[Union[Planet, AxialCusps]]] = Field(default=DEFAULT_ACTIVE_POINTS, description="The active points to display in the chart.", examples=[DEFAULT_ACTIVE_POINTS])
    active_aspects: Optional[list[ActiveAspect]] = Field(default=DEFAULT_ACTIVE_ASPECTS, description="The active aspects to display in the chart.", examples=[DEFAULT_ACTIVE_ASPECTS])

class TransitChartRequestModel(BaseModel):
    """
    The request model for the Transit Chart endpoint.
    """

    first_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    transit_subject: TransitSubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    theme: Optional[KerykeionChartTheme] = Field(default="classic", description="The theme of the chart.", examples=["classic", "light", "dark", "dark-high-contrast"])
    language: Optional[KerykeionChartLanguage] = Field(default="EN", description="The language of the chart.", examples=list(get_args(KerykeionChartLanguage)))
    wheel_only: Optional[bool] = Field(default=False, description="If set to True, only the zodiac wheel will be returned. No additional information will be displayed.")
    active_points: Optional[list[Union[Planet, AxialCusps]]] = Field(default=DEFAULT_ACTIVE_POINTS, description="The active points to display in the chart.", examples=[DEFAULT_ACTIVE_POINTS])
    active_aspects: Optional[list[ActiveAspect]] = Field(default=DEFAULT_ACTIVE_ASPECTS, description="The active aspects to display in the chart.", examples=[DEFAULT_ACTIVE_ASPECTS])

class BirthDataRequestModel(BaseModel):
    """
    The request model for the Birth Data endpoint.
    """

    subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")


class RelationshipScoreRequestModel(BaseModel):
    """
    The request model for the Relationship Score endpoint.
    """

    first_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    second_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")


class SynastryAspectsRequestModel(BaseModel):
    """
    The request model for the Aspects endpoint.
    """

    first_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    second_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    active_points: Optional[list[Union[Planet, AxialCusps]]] = Field(default=DEFAULT_ACTIVE_POINTS, description="The active points to display in the chart.", examples=[DEFAULT_ACTIVE_POINTS])
    active_aspects: Optional[list[ActiveAspect]] = Field(default=DEFAULT_ACTIVE_ASPECTS, description="The active aspects to display in the chart.", examples=[DEFAULT_ACTIVE_ASPECTS])

class NatalAspectsRequestModel(BaseModel):
    """
    The request model for the Birth Data endpoint.
    """

    subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    active_points: Optional[list[Union[Planet, AxialCusps]]] = Field(default=DEFAULT_ACTIVE_POINTS, description="The active points to display in the chart.", examples=[DEFAULT_ACTIVE_POINTS])
    active_aspects: Optional[list[ActiveAspect]] = Field(default=DEFAULT_ACTIVE_ASPECTS, description="The active aspects to display in the chart.", examples=[DEFAULT_ACTIVE_ASPECTS])


class CompositeChartRequestModel(BaseModel):
    """
    The request model for the Synastry Chart endpoint.
    """

    first_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    second_subject: SubjectModel = Field(description="The name of the person to get the Birth Chart for.")
    theme: Optional[KerykeionChartTheme] = Field(default="classic", description="The theme of the chart.", examples=["classic", "light", "dark", "dark-high-contrast"])
    language: Optional[KerykeionChartLanguage] = Field(default="EN", description="The language of the chart.", examples=list(get_args(KerykeionChartLanguage)))
    wheel_only: Optional[bool] = Field(default=False, description="If set to True, only the zodiac wheel will be returned. No additional information will be displayed.")
    active_points: Optional[list[Union[Planet, AxialCusps]]] = Field(default=DEFAULT_ACTIVE_POINTS, description="The active points to display in the chart.", examples=[DEFAULT_ACTIVE_POINTS])
    active_aspects: Optional[list[ActiveAspect]] = Field(default=DEFAULT_ACTIVE_ASPECTS, description="The active aspects to display in the chart.", examples=[DEFAULT_ACTIVE_ASPECTS])
````

## File: app/types/response_models.py
````python
from pydantic import BaseModel, Field

from kerykeion.kr_types import LunarPhaseModel, AstrologicalSubjectModel, CompositeSubjectModel
from kerykeion.kr_types import Quality, Element, Sign, Houses, Planet, AxialCusps, AspectName, SignsEmoji, SignNumbers, PointType, ZodiacType
from typing import Optional


class AspectModel(BaseModel):
    """
    The model for the aspects, similar to the one in the Kerykeion library.
    """
    p1_name: Planet | AxialCusps = Field(description="The name of the first planet.")
    p1_abs_pos: float = Field(description="The absolute position of the first planet.")
    p2_name: Planet | AxialCusps = Field(description="The name of the second planet.")
    p2_abs_pos: float = Field(description="The absolute position of the second planet.")
    aspect: AspectName = Field(description="The aspect between the two planets.")
    orbit: float = Field(description="The orbit between the two planets.")
    aspect_degrees: float = Field(description="The degrees of the aspect.")
    diff: float = Field(description="The difference between the two planets.")
    p1: int = Field(description="The id of the first planet.")
    p2: int = Field(description="The id of the second planet.")


class PlanetModel(BaseModel):
    """
    The model for the planets, similar to the one in the Kerykeion library.
    """

    name: Planet | AxialCusps = Field(description="The name of the planet.")
    quality: Quality = Field(description="The quality of the planet.")
    element: Element = Field(description="The element of the planet.")
    sign: Sign = Field(description="The sign in which the planet is located.")
    sign_num: SignNumbers = Field(description="The number of the sign in which the planet is located.")
    position: float = Field(description="The position of the planet inside the sign.")
    abs_pos: float = Field(description="The absolute position of the planet in the 360 degrees circle of the zodiac.")
    emoji: SignsEmoji = Field(description="The emoji of the sign in which the planet is located.")
    point_type: PointType = Field(description="The type of the point.")
    house: Optional[Houses] = Field(description="The house in which the planet is located.")
    retrograde: Optional[bool] = Field(default=None, description="The retrograde status of the planet.")


class BirthDataModel(BaseModel):
    """
    The model for the birth data.
    """

    name: str = Field(description="The name of the subject.")
    year: int = Field(description="Year of birth.")
    month: int = Field(description="Month of birth.")
    day: int = Field(description="Day of birth.")
    hour: int = Field(description="Hour of birth.")
    minute: int = Field(description="Minute of birth.")
    city: str = Field(description="City of birth.")
    nation: str = Field(description="Nation of birth.")
    lng: float = Field(description="Longitude of birth.")
    lat: float = Field(description="Latitude of birth.")
    tz_str: str = Field(description="Timezone of birth.")
    zodiac_type: ZodiacType = Field(description="The type of zodiac used.")
    local_time: str = Field(description="The local time of birth.")
    utc_time: str = Field(description="The UTC time of birth.")
    julian_day: float = Field(description="The Julian day of birth.")

    # Planets
    sun: PlanetModel = Field(description="The data of the Sun.")
    moon: PlanetModel = Field(description="The data of the Moon.")
    mercury: PlanetModel = Field(description="The data of Mercury.")
    venus: PlanetModel = Field(description="The data of Venus.")
    mars: PlanetModel = Field(description="The data of Mars.")
    jupiter: PlanetModel = Field(description="The data of Jupiter.")
    saturn: PlanetModel = Field(description="The data of Saturn.")
    uranus: PlanetModel = Field(description="The data of Uranus.")
    neptune: PlanetModel = Field(description="The data of Neptune.")
    pluto: PlanetModel = Field(description="The data of Pluto.")
    chiron: PlanetModel = Field(description="The data of Chiron.")

    # Axial Cusps
    asc: PlanetModel = Field(description="The data of the ascendant.")
    dsc: PlanetModel = Field(description="The data of the descendant.")
    mc: PlanetModel = Field(description="The data of the midheaven.")
    ic: PlanetModel = Field(description="The data of the imum coeli.")
    
    # Houses
    first_house: PlanetModel = Field(description="The data of the first house.")
    second_house: PlanetModel = Field(description="The data of the second house.")
    third_house: PlanetModel = Field(description="The data of the third house.")
    fourth_house: PlanetModel = Field(description="The data of the fourth house.")
    fifth_house: PlanetModel = Field(description="The data of the fifth house.")
    sixth_house: PlanetModel = Field(description="The data of the sixth house.")
    seventh_house: PlanetModel = Field(description="The data of the seventh house.")
    eighth_house: PlanetModel = Field(description="The data of the eighth house.")
    ninth_house: PlanetModel = Field(description="The data of the ninth house.")
    tenth_house: PlanetModel = Field(description="The data of the tenth house.")
    eleventh_house: PlanetModel = Field(description="The data of the eleventh house.")
    twelfth_house: PlanetModel = Field(description="The data of the twelfth house.")

    # Nodes
    mean_node: PlanetModel = Field(description="The data of the mean node.")
    true_node: PlanetModel = Field(description="The data of the true node.")

    # Lunar Phase
    lunar_phase: Optional[LunarPhaseModel] = Field(description="The lunar phase of the subject.")


class BirthDataResponseModel(BaseModel):
    """
    The response model for the Birth Data endpoint.
    """
    status: str = Field(description="The status of the response.")
    data: BirthDataModel = Field(description="The data of the subject.")


class BirthChartResponseModel(BaseModel):
    """
    The response model for the Birth Chart endpoint.
    """
    status: str = Field(description="The status of the response.")
    data: BirthDataModel = Field(description="The data of the subject.")
    chart: str = Field(description="The SVG chart of the birth chart.")
    aspects: list[AspectModel] = Field(description="The aspects of the birth chart.")


class DoubleDataModel(BaseModel):
    """
    The model for the data of two subjects.
    """
    first_subject: AstrologicalSubjectModel = Field(description="The data of the first subject.")
    second_subject: AstrologicalSubjectModel = Field(description="The data of the second subject.")


class TransitDataModel(BaseModel):
    """
    The model for the data of two subjects.
    """
    first_subject: AstrologicalSubjectModel = Field(description="The data of the first subject.")
    transit: AstrologicalSubjectModel = Field(description="The data of the second subject.")


class CompositeDataModel(BaseModel):
    """
    The model for the data of the composite chart.
    """
    composite_subject: CompositeSubjectModel = Field(description="The data of the composite chart.")
    first_subject: AstrologicalSubjectModel = Field(description="The data of the first subject.")
    second_subject: AstrologicalSubjectModel = Field(description="The data of the second subject.")


class SynastryChartResponseModel(BaseModel):
    """
    The response model for the Synastry.
    """
    status: str = Field(description="The status of the response.")
    data: DoubleDataModel = Field(description="The data of the two subjects.")
    chart: str = Field(description="The SVG chart of the synastry.")
    aspects: list[AspectModel] = Field(description="The aspects between the two subjects.")


class TransitChartResponseModel(BaseModel):
    """
    The response model for the Transit.
    """
    status: str = Field(description="The status of the response.")
    data: TransitDataModel = Field(description="The data of the two subjects.")
    chart: str = Field(description="The SVG chart of the transit.")
    aspects: list[AspectModel] = Field(description="The aspects between the two subjects.")


class TransitAspectsResponseModel(BaseModel):
    """
    The response model for the Transit Data endpoint.
    """
    status: str = Field(description="The status of the response.")
    data: TransitDataModel = Field(description="The data of the two subjects.")
    aspects: list[AspectModel] = Field(description="The aspects between the two subjects.")


class RelationshipScoreResponseModel(BaseModel):
    """
    The response model for the Relationship Score endpoint.
    """

    status: str = Field(description="The status of the response.")
    data: DoubleDataModel = Field(description="The data of the two subjects.")
    score: float = Field(description="The relationship score between the two subjects.")
    aspects: list[AspectModel] = Field(description="The aspects between the two subjects. In the Kerykeion library is referred as 'relevant_aspects'.")
    is_destiny_sign: bool = Field(description="If the two sings are reciprocally destiny signs.")


class SynastryAspectsResponseModel(BaseModel):
    """
    The response model for the Aspects endpoint.
    """

    status: str = Field(description="The status of the response.")
    data: DoubleDataModel = Field(description="The data of the two subjects.")
    aspects: list[AspectModel] = Field(description="A list with the aspects between the two subjects.")


class CompositeChartResponseModel(BaseModel):
    """
    The response model for the Composite Chart endpoint.
    """

    status: str = Field(description="The status of the response.")
    data: CompositeDataModel = Field(description="The data of the subjects and the composite chart.")
    chart: str = Field(description="The SVG chart of the composite chart.")
    aspects: list[AspectModel] = Field(description="The aspects between the two subjects.")


class CompositeAspectsResponseModel(BaseModel):
    """
    The response model for the Composite Aspects endpoint.
    """

    status: str = Field(description="The status of the response.")
    data: CompositeDataModel = Field(description="The data of the subjects and the composite chart.")
    aspects: list[AspectModel] = Field(description="A list with the aspects between the two subjects.")
````

## File: app/utils/get_ntp_time.py
````python
import socket
import struct
from typing import Union
from datetime import datetime, timezone
import logging

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def get_ntp_time(server: str = "time.google.com", timeout: int = 5) -> Union[datetime, Exception]:
    """
    Gets the current time from an NTP server.
    
    Args:
        server: The NTP server to use (default: time.google.com)
        timeout: The connection timeout in seconds (default: 5)
    
    Returns:
        A datetime object in UTC timezone representing the current time or raises an exception
    """
    NTP_PORT = 123
    # RFC 4330 format - Mode: 3 (client), Version: 3
    NTP_PACKET = b'\x1b' + 47 * b'\0'
    
    try:
        with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as sock:
            sock.settimeout(timeout)
            sock.sendto(NTP_PACKET, (server, NTP_PORT))
            data, _ = sock.recvfrom(48)
            
            # Extract the timestamp (second field Transmit Timestamp)
            # RFC 4330: bytes 40-47 contain the Transmit Timestamp
            transmit_time = struct.unpack('!II', data[40:48])
            
            # The first value represents seconds since 1900-01-01
            ntp_seconds = transmit_time[0]
            
            # Convert from NTP epoch (1900) to Unix epoch (1970)
            unix_time = ntp_seconds - 2208988800
            
            # Return a datetime object with UTC timezone
            return datetime.fromtimestamp(unix_time, tz=timezone.utc)
            
    except socket.timeout as e:
        logger.error(f"Timeout error: {e}")
        raise TimeoutError("Timeout during NTP request") from e
    except Exception as e:
        logger.error(f"An error occurred: {e}")
        raise e

if __name__ == "__main__":
    # Example usage
    try:
        ntp_time = get_ntp_time()
        print(f"Current NTP time: {ntp_time}")
    except Exception as e:
        print(f"Failed to get NTP time: {e}")
````

## File: app/utils/get_time_from_google.py
````python
import requests
from datetime import datetime

def get_time_from_google():
    response = requests.head("https://www.google.com")
    date_header = response.headers.get("Date")
    
    if date_header:
        # Converti la stringa della data in un oggetto datetime
        return datetime.strptime(date_header, "%a, %d %b %Y %H:%M:%S GMT")
    else:
        raise ValueError("Header 'Date' not found in the response.")

if __name__ == "__main__":
    current_time = get_time_from_google()
    print("Ora corrente da Google:", current_time)
````

## File: app/utils/internal_server_error_json_response.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""

from fastapi.responses import JSONResponse

InternalServerErrorJsonResponse = JSONResponse(
    status_code=500,
    content={
        "message": "Internal Server Error",
        "status": "KO",
    },
)
````

## File: app/utils/write_request_to_log.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""

from logging import Logger
from fastapi import Request


def get_write_request_to_log(logger: Logger):
    def write_request_to_log(level, request: Request, message: str | Exception):
        logger.log(level, f"{str(request.url)}: {str(message)}")

    return write_request_to_log
````

## File: app/__init__.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""
````

## File: app/main.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""

import logging
import logging.config

from fastapi import FastAPI

from .routers import main_router
from .config.settings import settings
from .middleware.secret_key_checker_middleware import SecretKeyCheckerMiddleware


logging.config.dictConfig(settings.LOGGING_CONFIG)
app = FastAPI(
    debug=settings.debug,
    docs_url=settings.docs_url,
    redoc_url=settings.redoc_url,
    title="Astrologer API",
    version="4.0.0",
    summary="Astrology Made Easy",
    description="The Astrologer API is a RESTful service providing extensive astrology calculations, designed for seamless integration into projects. It offers a rich set of astrological charts and data, making it an invaluable tool for both developers and astrology enthusiasts.",
    contact={
        "name": "Kerykeion Astrology",
        "url": "https://www.kerykeion.net/",
        "email": settings.admin_email,
    },
    license_info={
        "name": "AGPL-3.0",
        "url": "https://www.gnu.org/licenses/agpl-3.0.html",
    },
)

#------------------------------------------------------------------------------
# Routers 
#------------------------------------------------------------------------------

app.include_router(main_router.router, tags=["Endpoints"])

#------------------------------------------------------------------------------
# Middleware 
#------------------------------------------------------------------------------

if settings.debug is True:
    pass

else:
    app.add_middleware(
        SecretKeyCheckerMiddleware,
        secret_key_name=settings.secret_key_name,
        secret_keys=[
            settings.rapid_api_secret_key,
        ],
    )
````

## File: tests/test_main.py
````python
"""
    This is part of Astrologer API (C) 2023 Giacomo Battaglia
"""

from sys import path
from pathlib import Path

path.append(str(Path(__file__).parent.parent))

from fastapi.testclient import TestClient
from app.main import app
from datetime import datetime, timezone

client = TestClient(app)


def test_status():
    """
    Tests if the status endpoint returns the correct status.
    """

    response = client.get("/")

    assert response.status_code == 200
    assert response.json()["status"] == "OK"


def test_get_now():
    """
    Tests if the now function returns the correct utm time.
    """

    now = datetime.now(timezone.utc)
    response = client.get("/api/v4/now")

    assert response.status_code == 200
    assert response.json()["status"] == "OK"
    assert response.json()["data"]["name"] == "Now"
    assert response.json()["data"]["year"] == now.year
    assert response.json()["data"]["month"] == now.month
    assert response.json()["data"]["day"] == now.day
    assert response.json()["data"]["minute"] == now.minute


def test_birth_data():
    """Test if the birth data is returned correctly"""

    response = client.post(
        "/api/v4/birth-data",
        json={
            "subject": {
                "name": "FastAPI Unit Test",
                "year": 1946,
                "month": 6,
                "day": 16,
                "hour": 10,
                "minute": 10,
                "longitude": 12.4963655,
                "latitude": 41.9027835,
                "city": "Roma",
                "nation": "IT",
                "timezone": "Europe/Rome",
                "language": "IT",
            }
        },
    )

    assert response.status_code == 200
    assert response.json()["status"] == "OK"

    assert response.json()["data"]["nation"] == "IT"

    assert response.json()["data"]["sun"]["name"] == "Sun"
    assert response.json()["data"]["sun"]["quality"] == "Mutable"
    assert response.json()["data"]["sun"]["element"] == "Air"
    assert response.json()["data"]["sun"]["sign"] == "Gem"
    assert response.json()["data"]["sun"]["sign_num"] == 2
    assert round(response.json()["data"]["sun"]["position"]) == 25
    assert round(response.json()["data"]["sun"]["abs_pos"]) == 85
    assert response.json()["data"]["sun"]["emoji"] == "♊️"
    assert response.json()["data"]["sun"]["house"] == "Eleventh_House"
    assert response.json()["data"]["sun"]["retrograde"] == False
    assert response.json()["data"]["sun"]["point_type"] == "Planet"

    assert response.json()["data"]["lunar_phase"]["moon_emoji"] == "🌖"


def test_relationship_score():
    """
    Tests if the relationship score is returned correctly
    """

    response = client.post(
        "/api/v4/relationship-score",
        json={
            "first_subject": {
                "name": "FastAPI Unit Test",
                "year": 1946,
                "month": 6,
                "day": 16,
                "hour": 10,
                "minute": 10,
                "longitude": 12.4963655,
                "latitude": 41.9027835,
                "city": "Roma",
                "nation": "IT",
                "timezone": "Europe/Rome",
                "language": "IT",
            },
            "second_subject": {
                "name": "FastAPI Unit Test",
                "year": 1946,
                "month": 6,
                "day": 16,
                "hour": 10,
                "minute": 10,
                "longitude": 12.4963655,
                "latitude": 41.9027835,
                "city": "Roma",
                "nation": "IT",
                "timezone": "Europe/Rome",
                "language": "IT",
            },
        },
    )

    assert response.status_code == 200
    assert response.json()["status"] == "OK"
    assert response.json()["score"] == 24


def test_birth_chart():
    """
    Tests if the birth chart is returned correctly
    """

    response = client.post(
        "/api/v4/birth-chart",
        json={
            "subject": {
                "name": "FastAPI Unit Test",
                "year": 1980,
                "month": 12,
                "day": 12,
                "hour": 12,
                "minute": 12,
                "longitude": 0,
                "latitude": 51.4825766,
                "city": "London",
                "nation": "GB",
                "timezone": "Europe/London",
            }
        },
    )

    # ------------------
    # Status
    # ------------------

    assert response.status_code == 200
    assert response.json()["status"] == "OK"

    # ------------------
    # Data
    # ------------------

    ## Sun
    assert response.json()["data"]["sun"]["name"] == "Sun"
    assert response.json()["data"]["sun"]["quality"] == "Mutable"
    assert response.json()["data"]["sun"]["element"] == "Fire"
    assert response.json()["data"]["sun"]["sign"] == "Sag"
    assert response.json()["data"]["sun"]["sign_num"] == 8
    assert round(response.json()["data"]["sun"]["position"]) == 21
    assert round(response.json()["data"]["sun"]["abs_pos"]) == 261
    assert response.json()["data"]["sun"]["emoji"] == "♐️"
    assert response.json()["data"]["sun"]["point_type"] == "Planet"
    assert response.json()["data"]["sun"]["house"] == "Ninth_House"
    assert response.json()["data"]["sun"]["retrograde"] == False

    ## Moon Phase
    assert round(response.json()["data"]["lunar_phase"]["degrees_between_s_m"]) == 58
    assert response.json()["data"]["lunar_phase"]["moon_phase"] == 5
    assert response.json()["data"]["lunar_phase"]["sun_phase"] == 4
    assert response.json()["data"]["lunar_phase"]["moon_emoji"] == "🌒"

    # ------------------
    # Chart
    # ------------------

    assert type(response.json()["chart"]) == str

    # ------------------
    # Aspects
    # ------------------

    assert response.json()["aspects"][0]["p1_name"] == "Sun"
    assert round(response.json()["aspects"][0]["p1_abs_pos"]) == 261
    assert response.json()["aspects"][0]["p2_name"] == "Moon"
    assert round(response.json()["aspects"][0]["p2_abs_pos"]) == 318
    assert response.json()["aspects"][0]["aspect"] == "sextile"
    assert round(response.json()["aspects"][0]["orbit"]) == -2
    assert response.json()["aspects"][0]["aspect_degrees"] == 60
    assert round(response.json()["aspects"][0]["diff"]) == 58
    assert response.json()["aspects"][0]["p1"] == 0
    assert response.json()["aspects"][0]["p2"] == 1
````

## File: .editorconfig
````
root = true

[*.py]
indent_style = space
indent_size = 4
max_line_length = 120
````

## File: .gitignore
````
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
pip-wheel-metadata/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
*.py,cover
.hypothesis/
.pytest_cache/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
target/

# Jupyter Notebook
.ipynb_checkpoints

# IPython
profile_default/
ipython_config.py

# pyenv
.python-version

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock

# PEP 582; used by e.g. github.com/David-OConnor/pyflow
__pypackages__/

# Celery stuff
celerybeat-schedule
celerybeat.pid

# SageMath parsed files
*.sage.py

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# mkdocs documentation
/site

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre type checker
.pyre/
cache/geonames_cache.sqlite
cache/kerykeion_geonames_cache.sqlite
````

## File: dump_schema.py
````python
from app.main import app
import json

# with open("openapi.json", "w") as outfile:
#     json.dump(app.openapi(), outfile)

def dump_schema(output_file_path):
    BASE_URL = "https://astrologer.p.rapidapi.com/"
    RAPIDAPI_HOST = "astrologer.p.rapidapi.com" 
    openapi_data = app.openapi()

    # Define the rapidapi authentication headers
    rapidapi_auth = {
        "type": "apiKey",
        "name": "x-rapidapi-key",
        "in": "header"
    }

    # Add RapidAPI authentication to the securityDefinitions or components/securitySchemes
    if 'swagger' in openapi_data:  # Swagger 2.0
        openapi_data.setdefault('securityDefinitions', {})
        openapi_data['securityDefinitions']['RapidAPIKey'] = rapidapi_auth
    elif 'openapi' in openapi_data:  # OpenAPI 3.0+
        openapi_data.setdefault('components', {}).setdefault('securitySchemes', {})
        openapi_data['components']['securitySchemes']['RapidAPIKey'] = rapidapi_auth
    else:
        raise ValueError("Unrecognized OpenAPI version")

    # Apply the security scheme globally or to individual paths
    security_requirement = [{"RapidAPIKey": []}]
    
    # Add the security requirement to the paths
    if 'swagger' in openapi_data:  # Swagger 2.0
        for path in openapi_data.get('paths', {}).values():
            for method in path.values():
                method['security'] = security_requirement
    elif 'openapi' in openapi_data:  # OpenAPI 3.0+
        for path in openapi_data.get('paths', {}).values():
            for method in path.values():
                if isinstance(method, dict):  # Ensure we're modifying the correct level
                    method['security'] = security_requirement

    # Add the headers and hardcoded host/base URL to each path's operation
    for path, methods in openapi_data.get('paths', {}).items():
        for method, details in methods.items():
            if isinstance(details, dict):  # Ensure we're modifying the correct level
                details.setdefault('parameters', [])
                
                # Add x-rapidapi-key header
                details['parameters'].append({
                    "name": "x-rapidapi-key",
                    "in": "header",
                    "required": True,
                    "schema": {
                        "type": "string",
                        "example": "<YOUR_RAPIDAPI_KEY>"
                    }
                })
                
                # Hardcode x-rapidapi-host header
                details['parameters'].append({
                    "name": "x-rapidapi-host",
                    "in": "header",
                    "required": True,
                    "schema": {
                        "type": "string",
                        "example": RAPIDAPI_HOST
                    }
                })

                # Prepend base URL to all endpoints
                if 'servers' not in openapi_data:
                    openapi_data['servers'] = [{"url": BASE_URL}]
                else:
                    for server in openapi_data['servers']:
                        server['url'] = BASE_URL

    # Save the modified OpenAPI JSON file
    with open(output_file_path, 'w') as file:
        json.dump(openapi_data, file, indent=2)


if __name__ == "__main__":
    # https://editor-next.swagger.io/
    dump_schema("openapi.json")
    print("OpenAPI JSON file generated successfully! Check it here: https://editor-next.swagger.io/")
````

## File: openapi.json
````json
{
  "openapi": "3.1.0",
  "info": {
    "title": "Astrologer API",
    "summary": "Astrology Made Easy",
    "description": "The Astrologer API is a RESTful service providing extensive astrology calculations, designed for seamless integration into projects. It offers a rich set of astrological charts and data, making it an invaluable tool for both developers and astrology enthusiasts.",
    "contact": {
      "name": "Kerykeion Astrology",
      "url": "https://www.kerykeion.net/",
      "email": "kerykeion.astrology@gmail.com"
    },
    "license": {
      "name": "AGPL-3.0",
      "url": "https://www.gnu.org/licenses/agpl-3.0.html"
    },
    "version": "4.0.0"
  },
  "paths": {
    "/api/v4/now": {
      "get": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Get Now",
        "description": "Retrieve astrological data for the current moment.",
        "operationId": "get_now_api_v4_now_get",
        "responses": {
          "200": {
            "description": "Current astrological data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BirthDataResponseModel"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/birth-data": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Birth Data",
        "description": "Retrieve astrological data for a specific birth date. Does not include the chart nor the aspects.",
        "operationId": "birth_data_api_v4_birth_data_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BirthDataRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Birth data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BirthDataResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/birth-chart": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Birth Chart",
        "description": "Retrieve an astrological birth chart for a specific birth date. Includes the data for the subject and the aspects.",
        "operationId": "birth_chart_api_v4_birth_chart_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/BirthChartRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Birth chart",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/BirthChartResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/synastry-chart": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Synastry Chart",
        "description": "Retrieve a synastry chart between two subjects. Includes the data for the subjects and the aspects.",
        "operationId": "synastry_chart_api_v4_synastry_chart_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/SynastryChartRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Synastry data",
            "content": {
              "application/json": {
                "schema": {}
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/transit-chart": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Transit Chart",
        "description": "Retrieve a transit chart for a specific subject. Includes the data for the subject and the aspects.",
        "operationId": "transit_chart_api_v4_transit_chart_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/TransitChartRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Transit data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/TransitChartResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/transit-aspects-data": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Transit Aspects Data",
        "description": "Retrieve transit aspects and data for a specific subject. Does not include the chart.",
        "operationId": "transit_aspects_data_api_v4_transit_aspects_data_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/TransitChartRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Transit aspects data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/TransitAspectsResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/synastry-aspects-data": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Synastry Aspects Data",
        "description": "Retrieve synastry aspects between two subjects. Does not include the chart.",
        "operationId": "synastry_aspects_data_api_v4_synastry_aspects_data_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/SynastryAspectsRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Synastry aspects data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/SynastryAspectsResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/natal-aspects-data": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Natal Aspects Data",
        "description": "Retrieve natal aspects and data for a specific subject. Does not include the chart.",
        "operationId": "natal_aspects_data_api_v4_natal_aspects_data_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/NatalAspectsRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Birth aspects data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/SynastryAspectsResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/relationship-score": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Relationship Score",
        "description": "Calculates the relevance of the relationship between two subjects using the Ciro Discepolo method.\n\nResults:\n    - 0 to 5: Minimal relationship\n    - 5 to 10: Medium relationship\n    - 10 to 15: Important relationship\n    - 15 to 20: Very important relationship\n    - 20 to 35: Exceptional relationship\n    - 30 and above: Rare Exceptional relationship\n\nMore details: https://www-cirodiscepolo-it.translate.goog/Articoli/Discepoloele.htm?_x_tr_sl=it&_x_tr_tl=en&_x_tr_hl=it&_x_tr_pto=wapp",
        "operationId": "relationship_score_api_v4_relationship_score_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/RelationshipScoreRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Relationship score",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/RelationshipScoreResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/composite-chart": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Composite Chart",
        "description": "Retrieve a composite chart between two subjects. Includes the data for the subjects and the aspects.\nThe method used is the midpoint method.",
        "operationId": "composite_chart_api_v4_composite_chart_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CompositeChartRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Composite data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/CompositeChartResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    },
    "/api/v4/composite-aspects-data": {
      "post": {
        "tags": [
          "Endpoints"
        ],
        "summary": "Composite Aspects Data",
        "description": "Retrieves the data and the aspects for a composite chart between two subjects. Does not include the chart.",
        "operationId": "composite_aspects_data_api_v4_composite_aspects_data_post",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CompositeChartRequestModel"
              }
            }
          },
          "required": true
        },
        "responses": {
          "200": {
            "description": "Composite aspects data",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/CompositeAspectsResponseModel"
                }
              }
            }
          },
          "422": {
            "description": "Validation Error",
            "content": {
              "application/json": {
                "schema": {
                  "$ref": "#/components/schemas/HTTPValidationError"
                }
              }
            }
          }
        },
        "security": [
          {
            "RapidAPIKey": []
          }
        ],
        "parameters": [
          {
            "name": "x-rapidapi-key",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "<YOUR_RAPIDAPI_KEY>"
            }
          },
          {
            "name": "x-rapidapi-host",
            "in": "header",
            "required": true,
            "schema": {
              "type": "string",
              "example": "astrologer.p.rapidapi.com"
            }
          }
        ]
      }
    }
  },
  "components": {
    "schemas": {
      "ActiveAspect": {
        "properties": {
          "name": {
            "type": "string",
            "enum": [
              "conjunction",
              "semi-sextile",
              "semi-square",
              "sextile",
              "quintile",
              "square",
              "trine",
              "sesquiquadrate",
              "biquintile",
              "quincunx",
              "opposition"
            ],
            "title": "Name"
          },
          "orb": {
            "type": "integer",
            "title": "Orb"
          }
        },
        "type": "object",
        "required": [
          "name",
          "orb"
        ],
        "title": "ActiveAspect"
      },
      "AspectModel": {
        "properties": {
          "p1_name": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "Sun",
                  "Moon",
                  "Mercury",
                  "Venus",
                  "Mars",
                  "Jupiter",
                  "Saturn",
                  "Uranus",
                  "Neptune",
                  "Pluto",
                  "Mean_Node",
                  "True_Node",
                  "Mean_South_Node",
                  "True_South_Node",
                  "Chiron",
                  "Mean_Lilith"
                ]
              },
              {
                "type": "string",
                "enum": [
                  "Ascendant",
                  "Medium_Coeli",
                  "Descendant",
                  "Imum_Coeli"
                ]
              }
            ],
            "title": "P1 Name",
            "description": "The name of the first planet."
          },
          "p1_abs_pos": {
            "type": "number",
            "title": "P1 Abs Pos",
            "description": "The absolute position of the first planet."
          },
          "p2_name": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "Sun",
                  "Moon",
                  "Mercury",
                  "Venus",
                  "Mars",
                  "Jupiter",
                  "Saturn",
                  "Uranus",
                  "Neptune",
                  "Pluto",
                  "Mean_Node",
                  "True_Node",
                  "Mean_South_Node",
                  "True_South_Node",
                  "Chiron",
                  "Mean_Lilith"
                ]
              },
              {
                "type": "string",
                "enum": [
                  "Ascendant",
                  "Medium_Coeli",
                  "Descendant",
                  "Imum_Coeli"
                ]
              }
            ],
            "title": "P2 Name",
            "description": "The name of the second planet."
          },
          "p2_abs_pos": {
            "type": "number",
            "title": "P2 Abs Pos",
            "description": "The absolute position of the second planet."
          },
          "aspect": {
            "type": "string",
            "enum": [
              "conjunction",
              "semi-sextile",
              "semi-square",
              "sextile",
              "quintile",
              "square",
              "trine",
              "sesquiquadrate",
              "biquintile",
              "quincunx",
              "opposition"
            ],
            "title": "Aspect",
            "description": "The aspect between the two planets."
          },
          "orbit": {
            "type": "number",
            "title": "Orbit",
            "description": "The orbit between the two planets."
          },
          "aspect_degrees": {
            "type": "number",
            "title": "Aspect Degrees",
            "description": "The degrees of the aspect."
          },
          "diff": {
            "type": "number",
            "title": "Diff",
            "description": "The difference between the two planets."
          },
          "p1": {
            "type": "integer",
            "title": "P1",
            "description": "The id of the first planet."
          },
          "p2": {
            "type": "integer",
            "title": "P2",
            "description": "The id of the second planet."
          }
        },
        "type": "object",
        "required": [
          "p1_name",
          "p1_abs_pos",
          "p2_name",
          "p2_abs_pos",
          "aspect",
          "orbit",
          "aspect_degrees",
          "diff",
          "p1",
          "p2"
        ],
        "title": "AspectModel",
        "description": "The model for the aspects, similar to the one in the Kerykeion library."
      },
      "AstrologicalSubjectModel": {
        "properties": {
          "name": {
            "type": "string",
            "title": "Name"
          },
          "year": {
            "type": "integer",
            "title": "Year"
          },
          "month": {
            "type": "integer",
            "title": "Month"
          },
          "day": {
            "type": "integer",
            "title": "Day"
          },
          "hour": {
            "type": "integer",
            "title": "Hour"
          },
          "minute": {
            "type": "integer",
            "title": "Minute"
          },
          "city": {
            "type": "string",
            "title": "City"
          },
          "nation": {
            "type": "string",
            "title": "Nation"
          },
          "lng": {
            "type": "number",
            "title": "Lng"
          },
          "lat": {
            "type": "number",
            "title": "Lat"
          },
          "tz_str": {
            "type": "string",
            "title": "Tz Str"
          },
          "zodiac_type": {
            "type": "string",
            "enum": [
              "Tropic",
              "Sidereal"
            ],
            "title": "Zodiac Type"
          },
          "sidereal_mode": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "FAGAN_BRADLEY",
                  "LAHIRI",
                  "DELUCE",
                  "RAMAN",
                  "USHASHASHI",
                  "KRISHNAMURTI",
                  "DJWHAL_KHUL",
                  "YUKTESHWAR",
                  "JN_BHASIN",
                  "BABYL_KUGLER1",
                  "BABYL_KUGLER2",
                  "BABYL_KUGLER3",
                  "BABYL_HUBER",
                  "BABYL_ETPSC",
                  "ALDEBARAN_15TAU",
                  "HIPPARCHOS",
                  "SASSANIAN",
                  "J2000",
                  "J1900",
                  "B1950"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Sidereal Mode"
          },
          "houses_system_identifier": {
            "type": "string",
            "enum": [
              "A",
              "B",
              "C",
              "D",
              "F",
              "H",
              "I",
              "i",
              "K",
              "L",
              "M",
              "N",
              "O",
              "P",
              "Q",
              "R",
              "S",
              "T",
              "U",
              "V",
              "W",
              "X",
              "Y"
            ],
            "title": "Houses System Identifier"
          },
          "houses_system_name": {
            "type": "string",
            "title": "Houses System Name"
          },
          "perspective_type": {
            "type": "string",
            "enum": [
              "Apparent Geocentric",
              "Heliocentric",
              "Topocentric",
              "True Geocentric"
            ],
            "title": "Perspective Type"
          },
          "iso_formatted_local_datetime": {
            "type": "string",
            "title": "Iso Formatted Local Datetime"
          },
          "iso_formatted_utc_datetime": {
            "type": "string",
            "title": "Iso Formatted Utc Datetime"
          },
          "julian_day": {
            "type": "number",
            "title": "Julian Day"
          },
          "utc_time": {
            "type": "number",
            "title": "Utc Time"
          },
          "local_time": {
            "type": "number",
            "title": "Local Time"
          },
          "sun": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "moon": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mercury": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "venus": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mars": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "jupiter": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "saturn": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "uranus": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "neptune": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "pluto": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "ascendant": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "descendant": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "medium_coeli": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "imum_coeli": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "chiron": {
            "anyOf": [
              {
                "$ref": "#/components/schemas/KerykeionPointModel"
              },
              {
                "type": "null"
              }
            ]
          },
          "mean_lilith": {
            "anyOf": [
              {
                "$ref": "#/components/schemas/KerykeionPointModel"
              },
              {
                "type": "null"
              }
            ]
          },
          "first_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "second_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "third_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "fourth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "fifth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "sixth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "seventh_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "eighth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "ninth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "tenth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "eleventh_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "twelfth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mean_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "true_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mean_south_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "true_south_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "planets_names_list": {
            "items": {
              "type": "string",
              "enum": [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "True_Node",
                "Mean_South_Node",
                "True_South_Node",
                "Chiron",
                "Mean_Lilith"
              ]
            },
            "type": "array",
            "title": "Planets Names List"
          },
          "axial_cusps_names_list": {
            "items": {
              "type": "string",
              "enum": [
                "Ascendant",
                "Medium_Coeli",
                "Descendant",
                "Imum_Coeli"
              ]
            },
            "type": "array",
            "title": "Axial Cusps Names List"
          },
          "houses_names_list": {
            "items": {
              "type": "string",
              "enum": [
                "First_House",
                "Second_House",
                "Third_House",
                "Fourth_House",
                "Fifth_House",
                "Sixth_House",
                "Seventh_House",
                "Eighth_House",
                "Ninth_House",
                "Tenth_House",
                "Eleventh_House",
                "Twelfth_House"
              ]
            },
            "type": "array",
            "title": "Houses Names List"
          },
          "lunar_phase": {
            "$ref": "#/components/schemas/LunarPhaseModel"
          }
        },
        "type": "object",
        "required": [
          "name",
          "year",
          "month",
          "day",
          "hour",
          "minute",
          "city",
          "nation",
          "lng",
          "lat",
          "tz_str",
          "zodiac_type",
          "sidereal_mode",
          "houses_system_identifier",
          "houses_system_name",
          "perspective_type",
          "iso_formatted_local_datetime",
          "iso_formatted_utc_datetime",
          "julian_day",
          "utc_time",
          "local_time",
          "sun",
          "moon",
          "mercury",
          "venus",
          "mars",
          "jupiter",
          "saturn",
          "uranus",
          "neptune",
          "pluto",
          "ascendant",
          "descendant",
          "medium_coeli",
          "imum_coeli",
          "chiron",
          "mean_lilith",
          "first_house",
          "second_house",
          "third_house",
          "fourth_house",
          "fifth_house",
          "sixth_house",
          "seventh_house",
          "eighth_house",
          "ninth_house",
          "tenth_house",
          "eleventh_house",
          "twelfth_house",
          "mean_node",
          "true_node",
          "mean_south_node",
          "true_south_node",
          "planets_names_list",
          "axial_cusps_names_list",
          "houses_names_list",
          "lunar_phase"
        ],
        "title": "AstrologicalSubjectModel",
        "description": "Pydantic Model for Astrological Subject"
      },
      "BirthChartRequestModel": {
        "properties": {
          "subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "theme": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "light",
                  "dark",
                  "dark-high-contrast",
                  "classic"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Theme",
            "description": "The theme of the chart.",
            "default": "classic",
            "examples": [
              "classic",
              "light",
              "dark",
              "dark-high-contrast"
            ]
          },
          "language": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "EN",
                  "FR",
                  "PT",
                  "IT",
                  "CN",
                  "ES",
                  "RU",
                  "TR",
                  "DE",
                  "HI"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Language",
            "description": "The language of the chart.",
            "default": "EN",
            "examples": [
              "EN",
              "FR",
              "PT",
              "IT",
              "CN",
              "ES",
              "RU",
              "TR",
              "DE",
              "HI"
            ]
          },
          "wheel_only": {
            "anyOf": [
              {
                "type": "boolean"
              },
              {
                "type": "null"
              }
            ],
            "title": "Wheel Only",
            "description": "If set to True, only the zodiac wheel will be returned. No additional information will be displayed.",
            "default": false
          },
          "active_points": {
            "anyOf": [
              {
                "items": {
                  "anyOf": [
                    {
                      "type": "string",
                      "enum": [
                        "Sun",
                        "Moon",
                        "Mercury",
                        "Venus",
                        "Mars",
                        "Jupiter",
                        "Saturn",
                        "Uranus",
                        "Neptune",
                        "Pluto",
                        "Mean_Node",
                        "True_Node",
                        "Mean_South_Node",
                        "True_South_Node",
                        "Chiron",
                        "Mean_Lilith"
                      ]
                    },
                    {
                      "type": "string",
                      "enum": [
                        "Ascendant",
                        "Medium_Coeli",
                        "Descendant",
                        "Imum_Coeli"
                      ]
                    }
                  ]
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Points",
            "description": "The active points to display in the chart.",
            "default": [
              "Sun",
              "Moon",
              "Mercury",
              "Venus",
              "Mars",
              "Jupiter",
              "Saturn",
              "Uranus",
              "Neptune",
              "Pluto",
              "Mean_Node",
              "Chiron",
              "Ascendant",
              "Medium_Coeli",
              "Mean_Lilith",
              "Mean_South_Node"
            ],
            "examples": [
              [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "Chiron",
                "Ascendant",
                "Medium_Coeli",
                "Mean_Lilith",
                "Mean_South_Node"
              ]
            ]
          },
          "active_aspects": {
            "anyOf": [
              {
                "items": {
                  "$ref": "#/components/schemas/ActiveAspect"
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Aspects",
            "description": "The active aspects to display in the chart.",
            "default": [
              {
                "name": "conjunction",
                "orb": 10
              },
              {
                "name": "opposition",
                "orb": 10
              },
              {
                "name": "trine",
                "orb": 8
              },
              {
                "name": "sextile",
                "orb": 6
              },
              {
                "name": "square",
                "orb": 5
              },
              {
                "name": "quintile",
                "orb": 1
              }
            ],
            "examples": [
              [
                {
                  "name": "conjunction",
                  "orb": 10
                },
                {
                  "name": "opposition",
                  "orb": 10
                },
                {
                  "name": "trine",
                  "orb": 8
                },
                {
                  "name": "sextile",
                  "orb": 6
                },
                {
                  "name": "square",
                  "orb": 5
                },
                {
                  "name": "quintile",
                  "orb": 1
                }
              ]
            ]
          }
        },
        "type": "object",
        "required": [
          "subject"
        ],
        "title": "BirthChartRequestModel",
        "description": "The request model for the Birth Chart endpoint."
      },
      "BirthChartResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/BirthDataModel",
            "description": "The data of the subject."
          },
          "chart": {
            "type": "string",
            "title": "Chart",
            "description": "The SVG chart of the birth chart."
          },
          "aspects": {
            "items": {
              "$ref": "#/components/schemas/AspectModel"
            },
            "type": "array",
            "title": "Aspects",
            "description": "The aspects of the birth chart."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data",
          "chart",
          "aspects"
        ],
        "title": "BirthChartResponseModel",
        "description": "The response model for the Birth Chart endpoint."
      },
      "BirthDataModel": {
        "properties": {
          "name": {
            "type": "string",
            "title": "Name",
            "description": "The name of the subject."
          },
          "year": {
            "type": "integer",
            "title": "Year",
            "description": "Year of birth."
          },
          "month": {
            "type": "integer",
            "title": "Month",
            "description": "Month of birth."
          },
          "day": {
            "type": "integer",
            "title": "Day",
            "description": "Day of birth."
          },
          "hour": {
            "type": "integer",
            "title": "Hour",
            "description": "Hour of birth."
          },
          "minute": {
            "type": "integer",
            "title": "Minute",
            "description": "Minute of birth."
          },
          "city": {
            "type": "string",
            "title": "City",
            "description": "City of birth."
          },
          "nation": {
            "type": "string",
            "title": "Nation",
            "description": "Nation of birth."
          },
          "lng": {
            "type": "number",
            "title": "Lng",
            "description": "Longitude of birth."
          },
          "lat": {
            "type": "number",
            "title": "Lat",
            "description": "Latitude of birth."
          },
          "tz_str": {
            "type": "string",
            "title": "Tz Str",
            "description": "Timezone of birth."
          },
          "zodiac_type": {
            "type": "string",
            "enum": [
              "Tropic",
              "Sidereal"
            ],
            "title": "Zodiac Type",
            "description": "The type of zodiac used."
          },
          "local_time": {
            "type": "string",
            "title": "Local Time",
            "description": "The local time of birth."
          },
          "utc_time": {
            "type": "string",
            "title": "Utc Time",
            "description": "The UTC time of birth."
          },
          "julian_day": {
            "type": "number",
            "title": "Julian Day",
            "description": "The Julian day of birth."
          },
          "sun": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the Sun."
          },
          "moon": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the Moon."
          },
          "mercury": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Mercury."
          },
          "venus": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Venus."
          },
          "mars": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Mars."
          },
          "jupiter": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Jupiter."
          },
          "saturn": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Saturn."
          },
          "uranus": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Uranus."
          },
          "neptune": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Neptune."
          },
          "pluto": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Pluto."
          },
          "chiron": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of Chiron."
          },
          "asc": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the ascendant."
          },
          "dsc": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the descendant."
          },
          "mc": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the midheaven."
          },
          "ic": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the imum coeli."
          },
          "first_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the first house."
          },
          "second_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the second house."
          },
          "third_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the third house."
          },
          "fourth_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the fourth house."
          },
          "fifth_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the fifth house."
          },
          "sixth_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the sixth house."
          },
          "seventh_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the seventh house."
          },
          "eighth_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the eighth house."
          },
          "ninth_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the ninth house."
          },
          "tenth_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the tenth house."
          },
          "eleventh_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the eleventh house."
          },
          "twelfth_house": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the twelfth house."
          },
          "mean_node": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the mean node."
          },
          "true_node": {
            "$ref": "#/components/schemas/PlanetModel",
            "description": "The data of the true node."
          },
          "lunar_phase": {
            "anyOf": [
              {
                "$ref": "#/components/schemas/LunarPhaseModel"
              },
              {
                "type": "null"
              }
            ],
            "description": "The lunar phase of the subject."
          }
        },
        "type": "object",
        "required": [
          "name",
          "year",
          "month",
          "day",
          "hour",
          "minute",
          "city",
          "nation",
          "lng",
          "lat",
          "tz_str",
          "zodiac_type",
          "local_time",
          "utc_time",
          "julian_day",
          "sun",
          "moon",
          "mercury",
          "venus",
          "mars",
          "jupiter",
          "saturn",
          "uranus",
          "neptune",
          "pluto",
          "chiron",
          "asc",
          "dsc",
          "mc",
          "ic",
          "first_house",
          "second_house",
          "third_house",
          "fourth_house",
          "fifth_house",
          "sixth_house",
          "seventh_house",
          "eighth_house",
          "ninth_house",
          "tenth_house",
          "eleventh_house",
          "twelfth_house",
          "mean_node",
          "true_node",
          "lunar_phase"
        ],
        "title": "BirthDataModel",
        "description": "The model for the birth data."
      },
      "BirthDataRequestModel": {
        "properties": {
          "subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          }
        },
        "type": "object",
        "required": [
          "subject"
        ],
        "title": "BirthDataRequestModel",
        "description": "The request model for the Birth Data endpoint."
      },
      "BirthDataResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/BirthDataModel",
            "description": "The data of the subject."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data"
        ],
        "title": "BirthDataResponseModel",
        "description": "The response model for the Birth Data endpoint."
      },
      "CompositeAspectsResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/CompositeDataModel",
            "description": "The data of the subjects and the composite chart."
          },
          "aspects": {
            "items": {
              "$ref": "#/components/schemas/AspectModel"
            },
            "type": "array",
            "title": "Aspects",
            "description": "A list with the aspects between the two subjects."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data",
          "aspects"
        ],
        "title": "CompositeAspectsResponseModel",
        "description": "The response model for the Composite Aspects endpoint."
      },
      "CompositeChartRequestModel": {
        "properties": {
          "first_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "second_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "theme": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "light",
                  "dark",
                  "dark-high-contrast",
                  "classic"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Theme",
            "description": "The theme of the chart.",
            "default": "classic",
            "examples": [
              "classic",
              "light",
              "dark",
              "dark-high-contrast"
            ]
          },
          "language": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "EN",
                  "FR",
                  "PT",
                  "IT",
                  "CN",
                  "ES",
                  "RU",
                  "TR",
                  "DE",
                  "HI"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Language",
            "description": "The language of the chart.",
            "default": "EN",
            "examples": [
              "EN",
              "FR",
              "PT",
              "IT",
              "CN",
              "ES",
              "RU",
              "TR",
              "DE",
              "HI"
            ]
          },
          "wheel_only": {
            "anyOf": [
              {
                "type": "boolean"
              },
              {
                "type": "null"
              }
            ],
            "title": "Wheel Only",
            "description": "If set to True, only the zodiac wheel will be returned. No additional information will be displayed.",
            "default": false
          },
          "active_points": {
            "anyOf": [
              {
                "items": {
                  "anyOf": [
                    {
                      "type": "string",
                      "enum": [
                        "Sun",
                        "Moon",
                        "Mercury",
                        "Venus",
                        "Mars",
                        "Jupiter",
                        "Saturn",
                        "Uranus",
                        "Neptune",
                        "Pluto",
                        "Mean_Node",
                        "True_Node",
                        "Mean_South_Node",
                        "True_South_Node",
                        "Chiron",
                        "Mean_Lilith"
                      ]
                    },
                    {
                      "type": "string",
                      "enum": [
                        "Ascendant",
                        "Medium_Coeli",
                        "Descendant",
                        "Imum_Coeli"
                      ]
                    }
                  ]
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Points",
            "description": "The active points to display in the chart.",
            "default": [
              "Sun",
              "Moon",
              "Mercury",
              "Venus",
              "Mars",
              "Jupiter",
              "Saturn",
              "Uranus",
              "Neptune",
              "Pluto",
              "Mean_Node",
              "Chiron",
              "Ascendant",
              "Medium_Coeli",
              "Mean_Lilith",
              "Mean_South_Node"
            ],
            "examples": [
              [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "Chiron",
                "Ascendant",
                "Medium_Coeli",
                "Mean_Lilith",
                "Mean_South_Node"
              ]
            ]
          },
          "active_aspects": {
            "anyOf": [
              {
                "items": {
                  "$ref": "#/components/schemas/ActiveAspect"
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Aspects",
            "description": "The active aspects to display in the chart.",
            "default": [
              {
                "name": "conjunction",
                "orb": 10
              },
              {
                "name": "opposition",
                "orb": 10
              },
              {
                "name": "trine",
                "orb": 8
              },
              {
                "name": "sextile",
                "orb": 6
              },
              {
                "name": "square",
                "orb": 5
              },
              {
                "name": "quintile",
                "orb": 1
              }
            ],
            "examples": [
              [
                {
                  "name": "conjunction",
                  "orb": 10
                },
                {
                  "name": "opposition",
                  "orb": 10
                },
                {
                  "name": "trine",
                  "orb": 8
                },
                {
                  "name": "sextile",
                  "orb": 6
                },
                {
                  "name": "square",
                  "orb": 5
                },
                {
                  "name": "quintile",
                  "orb": 1
                }
              ]
            ]
          }
        },
        "type": "object",
        "required": [
          "first_subject",
          "second_subject"
        ],
        "title": "CompositeChartRequestModel",
        "description": "The request model for the Synastry Chart endpoint."
      },
      "CompositeChartResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/CompositeDataModel",
            "description": "The data of the subjects and the composite chart."
          },
          "chart": {
            "type": "string",
            "title": "Chart",
            "description": "The SVG chart of the composite chart."
          },
          "aspects": {
            "items": {
              "$ref": "#/components/schemas/AspectModel"
            },
            "type": "array",
            "title": "Aspects",
            "description": "The aspects between the two subjects."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data",
          "chart",
          "aspects"
        ],
        "title": "CompositeChartResponseModel",
        "description": "The response model for the Composite Chart endpoint."
      },
      "CompositeDataModel": {
        "properties": {
          "composite_subject": {
            "$ref": "#/components/schemas/CompositeSubjectModel",
            "description": "The data of the composite chart."
          },
          "first_subject": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel",
            "description": "The data of the first subject."
          },
          "second_subject": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel",
            "description": "The data of the second subject."
          }
        },
        "type": "object",
        "required": [
          "composite_subject",
          "first_subject",
          "second_subject"
        ],
        "title": "CompositeDataModel",
        "description": "The model for the data of the composite chart."
      },
      "CompositeSubjectModel": {
        "properties": {
          "name": {
            "type": "string",
            "title": "Name"
          },
          "first_subject": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel"
          },
          "second_subject": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel"
          },
          "composite_chart_type": {
            "type": "string",
            "title": "Composite Chart Type"
          },
          "zodiac_type": {
            "type": "string",
            "enum": [
              "Tropic",
              "Sidereal"
            ],
            "title": "Zodiac Type"
          },
          "sidereal_mode": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "FAGAN_BRADLEY",
                  "LAHIRI",
                  "DELUCE",
                  "RAMAN",
                  "USHASHASHI",
                  "KRISHNAMURTI",
                  "DJWHAL_KHUL",
                  "YUKTESHWAR",
                  "JN_BHASIN",
                  "BABYL_KUGLER1",
                  "BABYL_KUGLER2",
                  "BABYL_KUGLER3",
                  "BABYL_HUBER",
                  "BABYL_ETPSC",
                  "ALDEBARAN_15TAU",
                  "HIPPARCHOS",
                  "SASSANIAN",
                  "J2000",
                  "J1900",
                  "B1950"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Sidereal Mode"
          },
          "houses_system_identifier": {
            "type": "string",
            "enum": [
              "A",
              "B",
              "C",
              "D",
              "F",
              "H",
              "I",
              "i",
              "K",
              "L",
              "M",
              "N",
              "O",
              "P",
              "Q",
              "R",
              "S",
              "T",
              "U",
              "V",
              "W",
              "X",
              "Y"
            ],
            "title": "Houses System Identifier"
          },
          "houses_system_name": {
            "type": "string",
            "title": "Houses System Name"
          },
          "perspective_type": {
            "type": "string",
            "enum": [
              "Apparent Geocentric",
              "Heliocentric",
              "Topocentric",
              "True Geocentric"
            ],
            "title": "Perspective Type"
          },
          "sun": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "moon": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mercury": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "venus": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mars": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "jupiter": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "saturn": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "uranus": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "neptune": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "pluto": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "ascendant": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "descendant": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "medium_coeli": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "imum_coeli": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "chiron": {
            "anyOf": [
              {
                "$ref": "#/components/schemas/KerykeionPointModel"
              },
              {
                "type": "null"
              }
            ]
          },
          "mean_lilith": {
            "anyOf": [
              {
                "$ref": "#/components/schemas/KerykeionPointModel"
              },
              {
                "type": "null"
              }
            ]
          },
          "first_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "second_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "third_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "fourth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "fifth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "sixth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "seventh_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "eighth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "ninth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "tenth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "eleventh_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "twelfth_house": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mean_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "true_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "mean_south_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "true_south_node": {
            "$ref": "#/components/schemas/KerykeionPointModel"
          },
          "planets_names_list": {
            "items": {
              "type": "string",
              "enum": [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "True_Node",
                "Mean_South_Node",
                "True_South_Node",
                "Chiron",
                "Mean_Lilith"
              ]
            },
            "type": "array",
            "title": "Planets Names List"
          },
          "axial_cusps_names_list": {
            "items": {
              "type": "string",
              "enum": [
                "Ascendant",
                "Medium_Coeli",
                "Descendant",
                "Imum_Coeli"
              ]
            },
            "type": "array",
            "title": "Axial Cusps Names List"
          },
          "houses_names_list": {
            "items": {
              "type": "string",
              "enum": [
                "First_House",
                "Second_House",
                "Third_House",
                "Fourth_House",
                "Fifth_House",
                "Sixth_House",
                "Seventh_House",
                "Eighth_House",
                "Ninth_House",
                "Tenth_House",
                "Eleventh_House",
                "Twelfth_House"
              ]
            },
            "type": "array",
            "title": "Houses Names List"
          },
          "lunar_phase": {
            "$ref": "#/components/schemas/LunarPhaseModel"
          }
        },
        "type": "object",
        "required": [
          "name",
          "first_subject",
          "second_subject",
          "composite_chart_type",
          "zodiac_type",
          "sidereal_mode",
          "houses_system_identifier",
          "houses_system_name",
          "perspective_type",
          "sun",
          "moon",
          "mercury",
          "venus",
          "mars",
          "jupiter",
          "saturn",
          "uranus",
          "neptune",
          "pluto",
          "ascendant",
          "descendant",
          "medium_coeli",
          "imum_coeli",
          "chiron",
          "mean_lilith",
          "first_house",
          "second_house",
          "third_house",
          "fourth_house",
          "fifth_house",
          "sixth_house",
          "seventh_house",
          "eighth_house",
          "ninth_house",
          "tenth_house",
          "eleventh_house",
          "twelfth_house",
          "mean_node",
          "true_node",
          "mean_south_node",
          "true_south_node",
          "planets_names_list",
          "axial_cusps_names_list",
          "houses_names_list",
          "lunar_phase"
        ],
        "title": "CompositeSubjectModel",
        "description": "Pydantic Model for Composite Subject"
      },
      "DoubleDataModel": {
        "properties": {
          "first_subject": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel",
            "description": "The data of the first subject."
          },
          "second_subject": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel",
            "description": "The data of the second subject."
          }
        },
        "type": "object",
        "required": [
          "first_subject",
          "second_subject"
        ],
        "title": "DoubleDataModel",
        "description": "The model for the data of two subjects."
      },
      "HTTPValidationError": {
        "properties": {
          "detail": {
            "items": {
              "$ref": "#/components/schemas/ValidationError"
            },
            "type": "array",
            "title": "Detail"
          }
        },
        "type": "object",
        "title": "HTTPValidationError"
      },
      "KerykeionPointModel": {
        "properties": {
          "name": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "Sun",
                  "Moon",
                  "Mercury",
                  "Venus",
                  "Mars",
                  "Jupiter",
                  "Saturn",
                  "Uranus",
                  "Neptune",
                  "Pluto",
                  "Mean_Node",
                  "True_Node",
                  "Mean_South_Node",
                  "True_South_Node",
                  "Chiron",
                  "Mean_Lilith"
                ]
              },
              {
                "type": "string",
                "enum": [
                  "First_House",
                  "Second_House",
                  "Third_House",
                  "Fourth_House",
                  "Fifth_House",
                  "Sixth_House",
                  "Seventh_House",
                  "Eighth_House",
                  "Ninth_House",
                  "Tenth_House",
                  "Eleventh_House",
                  "Twelfth_House"
                ]
              },
              {
                "type": "string",
                "enum": [
                  "Ascendant",
                  "Medium_Coeli",
                  "Descendant",
                  "Imum_Coeli"
                ]
              }
            ],
            "title": "Name"
          },
          "quality": {
            "type": "string",
            "enum": [
              "Cardinal",
              "Fixed",
              "Mutable"
            ],
            "title": "Quality"
          },
          "element": {
            "type": "string",
            "enum": [
              "Air",
              "Fire",
              "Earth",
              "Water"
            ],
            "title": "Element"
          },
          "sign": {
            "type": "string",
            "enum": [
              "Ari",
              "Tau",
              "Gem",
              "Can",
              "Leo",
              "Vir",
              "Lib",
              "Sco",
              "Sag",
              "Cap",
              "Aqu",
              "Pis"
            ],
            "title": "Sign"
          },
          "sign_num": {
            "type": "integer",
            "enum": [
              0,
              1,
              2,
              3,
              4,
              5,
              6,
              7,
              8,
              9,
              10,
              11
            ],
            "title": "Sign Num"
          },
          "position": {
            "type": "number",
            "title": "Position"
          },
          "abs_pos": {
            "type": "number",
            "title": "Abs Pos"
          },
          "emoji": {
            "type": "string",
            "title": "Emoji"
          },
          "point_type": {
            "type": "string",
            "enum": [
              "Planet",
              "House",
              "AxialCusps"
            ],
            "title": "Point Type"
          },
          "house": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "First_House",
                  "Second_House",
                  "Third_House",
                  "Fourth_House",
                  "Fifth_House",
                  "Sixth_House",
                  "Seventh_House",
                  "Eighth_House",
                  "Ninth_House",
                  "Tenth_House",
                  "Eleventh_House",
                  "Twelfth_House"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "House"
          },
          "retrograde": {
            "anyOf": [
              {
                "type": "boolean"
              },
              {
                "type": "null"
              }
            ],
            "title": "Retrograde"
          }
        },
        "type": "object",
        "required": [
          "name",
          "quality",
          "element",
          "sign",
          "sign_num",
          "position",
          "abs_pos",
          "emoji",
          "point_type"
        ],
        "title": "KerykeionPointModel",
        "description": "Kerykeion Point Model"
      },
      "LunarPhaseModel": {
        "properties": {
          "degrees_between_s_m": {
            "anyOf": [
              {
                "type": "number"
              },
              {
                "type": "integer"
              }
            ],
            "title": "Degrees Between S M"
          },
          "moon_phase": {
            "type": "integer",
            "title": "Moon Phase"
          },
          "sun_phase": {
            "type": "integer",
            "title": "Sun Phase"
          },
          "moon_emoji": {
            "type": "string",
            "enum": [
              "\ud83c\udf11",
              "\ud83c\udf12",
              "\ud83c\udf13",
              "\ud83c\udf14",
              "\ud83c\udf15",
              "\ud83c\udf16",
              "\ud83c\udf17",
              "\ud83c\udf18"
            ],
            "title": "Moon Emoji"
          },
          "moon_phase_name": {
            "type": "string",
            "enum": [
              "New Moon",
              "Waxing Crescent",
              "First Quarter",
              "Waxing Gibbous",
              "Full Moon",
              "Waning Gibbous",
              "Last Quarter",
              "Waning Crescent"
            ],
            "title": "Moon Phase Name"
          }
        },
        "type": "object",
        "required": [
          "degrees_between_s_m",
          "moon_phase",
          "sun_phase",
          "moon_emoji",
          "moon_phase_name"
        ],
        "title": "LunarPhaseModel"
      },
      "NatalAspectsRequestModel": {
        "properties": {
          "subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "active_points": {
            "anyOf": [
              {
                "items": {
                  "anyOf": [
                    {
                      "type": "string",
                      "enum": [
                        "Sun",
                        "Moon",
                        "Mercury",
                        "Venus",
                        "Mars",
                        "Jupiter",
                        "Saturn",
                        "Uranus",
                        "Neptune",
                        "Pluto",
                        "Mean_Node",
                        "True_Node",
                        "Mean_South_Node",
                        "True_South_Node",
                        "Chiron",
                        "Mean_Lilith"
                      ]
                    },
                    {
                      "type": "string",
                      "enum": [
                        "Ascendant",
                        "Medium_Coeli",
                        "Descendant",
                        "Imum_Coeli"
                      ]
                    }
                  ]
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Points",
            "description": "The active points to display in the chart.",
            "default": [
              "Sun",
              "Moon",
              "Mercury",
              "Venus",
              "Mars",
              "Jupiter",
              "Saturn",
              "Uranus",
              "Neptune",
              "Pluto",
              "Mean_Node",
              "Chiron",
              "Ascendant",
              "Medium_Coeli",
              "Mean_Lilith",
              "Mean_South_Node"
            ],
            "examples": [
              [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "Chiron",
                "Ascendant",
                "Medium_Coeli",
                "Mean_Lilith",
                "Mean_South_Node"
              ]
            ]
          },
          "active_aspects": {
            "anyOf": [
              {
                "items": {
                  "$ref": "#/components/schemas/ActiveAspect"
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Aspects",
            "description": "The active aspects to display in the chart.",
            "default": [
              {
                "name": "conjunction",
                "orb": 10
              },
              {
                "name": "opposition",
                "orb": 10
              },
              {
                "name": "trine",
                "orb": 8
              },
              {
                "name": "sextile",
                "orb": 6
              },
              {
                "name": "square",
                "orb": 5
              },
              {
                "name": "quintile",
                "orb": 1
              }
            ],
            "examples": [
              [
                {
                  "name": "conjunction",
                  "orb": 10
                },
                {
                  "name": "opposition",
                  "orb": 10
                },
                {
                  "name": "trine",
                  "orb": 8
                },
                {
                  "name": "sextile",
                  "orb": 6
                },
                {
                  "name": "square",
                  "orb": 5
                },
                {
                  "name": "quintile",
                  "orb": 1
                }
              ]
            ]
          }
        },
        "type": "object",
        "required": [
          "subject"
        ],
        "title": "NatalAspectsRequestModel",
        "description": "The request model for the Birth Data endpoint."
      },
      "PlanetModel": {
        "properties": {
          "name": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "Sun",
                  "Moon",
                  "Mercury",
                  "Venus",
                  "Mars",
                  "Jupiter",
                  "Saturn",
                  "Uranus",
                  "Neptune",
                  "Pluto",
                  "Mean_Node",
                  "True_Node",
                  "Mean_South_Node",
                  "True_South_Node",
                  "Chiron",
                  "Mean_Lilith"
                ]
              },
              {
                "type": "string",
                "enum": [
                  "Ascendant",
                  "Medium_Coeli",
                  "Descendant",
                  "Imum_Coeli"
                ]
              }
            ],
            "title": "Name",
            "description": "The name of the planet."
          },
          "quality": {
            "type": "string",
            "enum": [
              "Cardinal",
              "Fixed",
              "Mutable"
            ],
            "title": "Quality",
            "description": "The quality of the planet."
          },
          "element": {
            "type": "string",
            "enum": [
              "Air",
              "Fire",
              "Earth",
              "Water"
            ],
            "title": "Element",
            "description": "The element of the planet."
          },
          "sign": {
            "type": "string",
            "enum": [
              "Ari",
              "Tau",
              "Gem",
              "Can",
              "Leo",
              "Vir",
              "Lib",
              "Sco",
              "Sag",
              "Cap",
              "Aqu",
              "Pis"
            ],
            "title": "Sign",
            "description": "The sign in which the planet is located."
          },
          "sign_num": {
            "type": "integer",
            "enum": [
              0,
              1,
              2,
              3,
              4,
              5,
              6,
              7,
              8,
              9,
              10,
              11
            ],
            "title": "Sign Num",
            "description": "The number of the sign in which the planet is located."
          },
          "position": {
            "type": "number",
            "title": "Position",
            "description": "The position of the planet inside the sign."
          },
          "abs_pos": {
            "type": "number",
            "title": "Abs Pos",
            "description": "The absolute position of the planet in the 360 degrees circle of the zodiac."
          },
          "emoji": {
            "type": "string",
            "enum": [
              "\u2648\ufe0f",
              "\u2649\ufe0f",
              "\u264a\ufe0f",
              "\u264b\ufe0f",
              "\u264c\ufe0f",
              "\u264d\ufe0f",
              "\u264e\ufe0f",
              "\u264f\ufe0f",
              "\u2650\ufe0f",
              "\u2651\ufe0f",
              "\u2652\ufe0f",
              "\u2653\ufe0f"
            ],
            "title": "Emoji",
            "description": "The emoji of the sign in which the planet is located."
          },
          "point_type": {
            "type": "string",
            "enum": [
              "Planet",
              "House",
              "AxialCusps"
            ],
            "title": "Point Type",
            "description": "The type of the point."
          },
          "house": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "First_House",
                  "Second_House",
                  "Third_House",
                  "Fourth_House",
                  "Fifth_House",
                  "Sixth_House",
                  "Seventh_House",
                  "Eighth_House",
                  "Ninth_House",
                  "Tenth_House",
                  "Eleventh_House",
                  "Twelfth_House"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "House",
            "description": "The house in which the planet is located."
          },
          "retrograde": {
            "anyOf": [
              {
                "type": "boolean"
              },
              {
                "type": "null"
              }
            ],
            "title": "Retrograde",
            "description": "The retrograde status of the planet."
          }
        },
        "type": "object",
        "required": [
          "name",
          "quality",
          "element",
          "sign",
          "sign_num",
          "position",
          "abs_pos",
          "emoji",
          "point_type",
          "house"
        ],
        "title": "PlanetModel",
        "description": "The model for the planets, similar to the one in the Kerykeion library."
      },
      "RelationshipScoreRequestModel": {
        "properties": {
          "first_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "second_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          }
        },
        "type": "object",
        "required": [
          "first_subject",
          "second_subject"
        ],
        "title": "RelationshipScoreRequestModel",
        "description": "The request model for the Relationship Score endpoint."
      },
      "RelationshipScoreResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/DoubleDataModel",
            "description": "The data of the two subjects."
          },
          "score": {
            "type": "number",
            "title": "Score",
            "description": "The relationship score between the two subjects."
          },
          "aspects": {
            "items": {
              "$ref": "#/components/schemas/AspectModel"
            },
            "type": "array",
            "title": "Aspects",
            "description": "The aspects between the two subjects. In the Kerykeion library is referred as 'relevant_aspects'."
          },
          "is_destiny_sign": {
            "type": "boolean",
            "title": "Is Destiny Sign",
            "description": "If the two sings are reciprocally destiny signs."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data",
          "score",
          "aspects",
          "is_destiny_sign"
        ],
        "title": "RelationshipScoreResponseModel",
        "description": "The response model for the Relationship Score endpoint."
      },
      "SubjectModel": {
        "properties": {
          "year": {
            "type": "integer",
            "title": "Year",
            "description": "The year of birth.",
            "examples": [
              1980
            ]
          },
          "month": {
            "type": "integer",
            "title": "Month",
            "description": "The month of birth.",
            "examples": [
              12
            ]
          },
          "day": {
            "type": "integer",
            "title": "Day",
            "description": "The day of birth.",
            "examples": [
              12
            ]
          },
          "hour": {
            "type": "integer",
            "title": "Hour",
            "description": "The hour of birth.",
            "examples": [
              12
            ]
          },
          "minute": {
            "type": "integer",
            "title": "Minute",
            "description": "The minute of birth.",
            "examples": [
              12
            ]
          },
          "longitude": {
            "anyOf": [
              {
                "type": "number"
              },
              {
                "type": "null"
              }
            ],
            "title": "Longitude",
            "description": "The longitude of the birth location. Defaults on London.",
            "examples": [
              0
            ]
          },
          "latitude": {
            "anyOf": [
              {
                "type": "number"
              },
              {
                "type": "null"
              }
            ],
            "title": "Latitude",
            "description": "The latitude of the birth location. Defaults on London.",
            "examples": [
              51.4825766
            ]
          },
          "city": {
            "type": "string",
            "title": "City",
            "description": "The name of city of birth.",
            "examples": [
              "London"
            ]
          },
          "nation": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "null"
              }
            ],
            "title": "Nation",
            "description": "The name of the nation of birth.",
            "default": "null",
            "examples": [
              "GB"
            ]
          },
          "timezone": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "null"
              }
            ],
            "title": "Timezone",
            "description": "The timezone of the birth location.",
            "examples": [
              "Europe/London"
            ]
          },
          "geonames_username": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "null"
              }
            ],
            "title": "Geonames Username",
            "description": "The username for the Geonames API.",
            "examples": [
              null
            ]
          },
          "name": {
            "type": "string",
            "title": "Name",
            "description": "The name of the person to get the Birth Chart for.",
            "examples": [
              "John Doe"
            ]
          },
          "zodiac_type": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "Tropic",
                  "Sidereal"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Zodiac Type",
            "description": "The type of zodiac used (Tropic or Sidereal).",
            "default": "Tropic",
            "examples": [
              "Tropic",
              "Sidereal"
            ]
          },
          "sidereal_mode": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "FAGAN_BRADLEY",
                  "LAHIRI",
                  "DELUCE",
                  "RAMAN",
                  "USHASHASHI",
                  "KRISHNAMURTI",
                  "DJWHAL_KHUL",
                  "YUKTESHWAR",
                  "JN_BHASIN",
                  "BABYL_KUGLER1",
                  "BABYL_KUGLER2",
                  "BABYL_KUGLER3",
                  "BABYL_HUBER",
                  "BABYL_ETPSC",
                  "ALDEBARAN_15TAU",
                  "HIPPARCHOS",
                  "SASSANIAN",
                  "J2000",
                  "J1900",
                  "B1950"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Sidereal Mode",
            "description": "The sidereal mode used.",
            "examples": [
              null
            ]
          },
          "perspective_type": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "Apparent Geocentric",
                  "Heliocentric",
                  "Topocentric",
                  "True Geocentric"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Perspective Type",
            "description": "The perspective type used.",
            "default": "Apparent Geocentric",
            "examples": [
              "Apparent Geocentric",
              "Heliocentric",
              "Topocentric",
              "True Geocentric"
            ]
          },
          "houses_system_identifier": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "A",
                  "B",
                  "C",
                  "D",
                  "F",
                  "H",
                  "I",
                  "i",
                  "K",
                  "L",
                  "M",
                  "N",
                  "O",
                  "P",
                  "Q",
                  "R",
                  "S",
                  "T",
                  "U",
                  "V",
                  "W",
                  "X",
                  "Y"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Houses System Identifier",
            "description": "The house system to use. The following are the available house systems: A = equal B = Alcabitius C = Campanus D = equal (MC) F = Carter poli-equ. H = horizon/azimut I = Sunshine i = Sunshine/alt. K = Koch L = Pullen SD M = Morinus N = equal/1=Aries O = Porphyry P = Placidus Q = Pullen SR R = Regiomontanus S = Sripati T = Polich/Page U = Krusinski-Pisa-Goelzer V = equal/Vehlow W = equal/whole sign X = axial rotation system/Meridian houses Y = APC houses Usually the standard is Placidus (P)",
            "default": "P",
            "examples": [
              "P"
            ]
          }
        },
        "type": "object",
        "required": [
          "year",
          "month",
          "day",
          "hour",
          "minute",
          "city",
          "name"
        ],
        "title": "SubjectModel",
        "description": "The request model for the Birth Chart endpoint."
      },
      "SynastryAspectsRequestModel": {
        "properties": {
          "first_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "second_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "active_points": {
            "anyOf": [
              {
                "items": {
                  "anyOf": [
                    {
                      "type": "string",
                      "enum": [
                        "Sun",
                        "Moon",
                        "Mercury",
                        "Venus",
                        "Mars",
                        "Jupiter",
                        "Saturn",
                        "Uranus",
                        "Neptune",
                        "Pluto",
                        "Mean_Node",
                        "True_Node",
                        "Mean_South_Node",
                        "True_South_Node",
                        "Chiron",
                        "Mean_Lilith"
                      ]
                    },
                    {
                      "type": "string",
                      "enum": [
                        "Ascendant",
                        "Medium_Coeli",
                        "Descendant",
                        "Imum_Coeli"
                      ]
                    }
                  ]
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Points",
            "description": "The active points to display in the chart.",
            "default": [
              "Sun",
              "Moon",
              "Mercury",
              "Venus",
              "Mars",
              "Jupiter",
              "Saturn",
              "Uranus",
              "Neptune",
              "Pluto",
              "Mean_Node",
              "Chiron",
              "Ascendant",
              "Medium_Coeli",
              "Mean_Lilith",
              "Mean_South_Node"
            ],
            "examples": [
              [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "Chiron",
                "Ascendant",
                "Medium_Coeli",
                "Mean_Lilith",
                "Mean_South_Node"
              ]
            ]
          },
          "active_aspects": {
            "anyOf": [
              {
                "items": {
                  "$ref": "#/components/schemas/ActiveAspect"
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Aspects",
            "description": "The active aspects to display in the chart.",
            "default": [
              {
                "name": "conjunction",
                "orb": 10
              },
              {
                "name": "opposition",
                "orb": 10
              },
              {
                "name": "trine",
                "orb": 8
              },
              {
                "name": "sextile",
                "orb": 6
              },
              {
                "name": "square",
                "orb": 5
              },
              {
                "name": "quintile",
                "orb": 1
              }
            ],
            "examples": [
              [
                {
                  "name": "conjunction",
                  "orb": 10
                },
                {
                  "name": "opposition",
                  "orb": 10
                },
                {
                  "name": "trine",
                  "orb": 8
                },
                {
                  "name": "sextile",
                  "orb": 6
                },
                {
                  "name": "square",
                  "orb": 5
                },
                {
                  "name": "quintile",
                  "orb": 1
                }
              ]
            ]
          }
        },
        "type": "object",
        "required": [
          "first_subject",
          "second_subject"
        ],
        "title": "SynastryAspectsRequestModel",
        "description": "The request model for the Aspects endpoint."
      },
      "SynastryAspectsResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/DoubleDataModel",
            "description": "The data of the two subjects."
          },
          "aspects": {
            "items": {
              "$ref": "#/components/schemas/AspectModel"
            },
            "type": "array",
            "title": "Aspects",
            "description": "A list with the aspects between the two subjects."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data",
          "aspects"
        ],
        "title": "SynastryAspectsResponseModel",
        "description": "The response model for the Aspects endpoint."
      },
      "SynastryChartRequestModel": {
        "properties": {
          "first_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "second_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "theme": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "light",
                  "dark",
                  "dark-high-contrast",
                  "classic"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Theme",
            "description": "The theme of the chart.",
            "default": "classic",
            "examples": [
              "classic",
              "light",
              "dark",
              "dark-high-contrast"
            ]
          },
          "language": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "EN",
                  "FR",
                  "PT",
                  "IT",
                  "CN",
                  "ES",
                  "RU",
                  "TR",
                  "DE",
                  "HI"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Language",
            "description": "The language of the chart.",
            "default": "EN",
            "examples": [
              "EN",
              "FR",
              "PT",
              "IT",
              "CN",
              "ES",
              "RU",
              "TR",
              "DE",
              "HI"
            ]
          },
          "wheel_only": {
            "anyOf": [
              {
                "type": "boolean"
              },
              {
                "type": "null"
              }
            ],
            "title": "Wheel Only",
            "description": "If set to True, only the zodiac wheel will be returned. No additional information will be displayed.",
            "default": false
          },
          "active_points": {
            "anyOf": [
              {
                "items": {
                  "anyOf": [
                    {
                      "type": "string",
                      "enum": [
                        "Sun",
                        "Moon",
                        "Mercury",
                        "Venus",
                        "Mars",
                        "Jupiter",
                        "Saturn",
                        "Uranus",
                        "Neptune",
                        "Pluto",
                        "Mean_Node",
                        "True_Node",
                        "Mean_South_Node",
                        "True_South_Node",
                        "Chiron",
                        "Mean_Lilith"
                      ]
                    },
                    {
                      "type": "string",
                      "enum": [
                        "Ascendant",
                        "Medium_Coeli",
                        "Descendant",
                        "Imum_Coeli"
                      ]
                    }
                  ]
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Points",
            "description": "The active points to display in the chart.",
            "default": [
              "Sun",
              "Moon",
              "Mercury",
              "Venus",
              "Mars",
              "Jupiter",
              "Saturn",
              "Uranus",
              "Neptune",
              "Pluto",
              "Mean_Node",
              "Chiron",
              "Ascendant",
              "Medium_Coeli",
              "Mean_Lilith",
              "Mean_South_Node"
            ],
            "examples": [
              [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "Chiron",
                "Ascendant",
                "Medium_Coeli",
                "Mean_Lilith",
                "Mean_South_Node"
              ]
            ]
          },
          "active_aspects": {
            "anyOf": [
              {
                "items": {
                  "$ref": "#/components/schemas/ActiveAspect"
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Aspects",
            "description": "The active aspects to display in the chart.",
            "default": [
              {
                "name": "conjunction",
                "orb": 10
              },
              {
                "name": "opposition",
                "orb": 10
              },
              {
                "name": "trine",
                "orb": 8
              },
              {
                "name": "sextile",
                "orb": 6
              },
              {
                "name": "square",
                "orb": 5
              },
              {
                "name": "quintile",
                "orb": 1
              }
            ],
            "examples": [
              [
                {
                  "name": "conjunction",
                  "orb": 10
                },
                {
                  "name": "opposition",
                  "orb": 10
                },
                {
                  "name": "trine",
                  "orb": 8
                },
                {
                  "name": "sextile",
                  "orb": 6
                },
                {
                  "name": "square",
                  "orb": 5
                },
                {
                  "name": "quintile",
                  "orb": 1
                }
              ]
            ]
          }
        },
        "type": "object",
        "required": [
          "first_subject",
          "second_subject"
        ],
        "title": "SynastryChartRequestModel",
        "description": "The request model for the Synastry Chart endpoint."
      },
      "TransitAspectsResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/TransitDataModel",
            "description": "The data of the two subjects."
          },
          "aspects": {
            "items": {
              "$ref": "#/components/schemas/AspectModel"
            },
            "type": "array",
            "title": "Aspects",
            "description": "The aspects between the two subjects."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data",
          "aspects"
        ],
        "title": "TransitAspectsResponseModel",
        "description": "The response model for the Transit Data endpoint."
      },
      "TransitChartRequestModel": {
        "properties": {
          "first_subject": {
            "$ref": "#/components/schemas/SubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "transit_subject": {
            "$ref": "#/components/schemas/TransitSubjectModel",
            "description": "The name of the person to get the Birth Chart for."
          },
          "theme": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "light",
                  "dark",
                  "dark-high-contrast",
                  "classic"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Theme",
            "description": "The theme of the chart.",
            "default": "classic",
            "examples": [
              "classic",
              "light",
              "dark",
              "dark-high-contrast"
            ]
          },
          "language": {
            "anyOf": [
              {
                "type": "string",
                "enum": [
                  "EN",
                  "FR",
                  "PT",
                  "IT",
                  "CN",
                  "ES",
                  "RU",
                  "TR",
                  "DE",
                  "HI"
                ]
              },
              {
                "type": "null"
              }
            ],
            "title": "Language",
            "description": "The language of the chart.",
            "default": "EN",
            "examples": [
              "EN",
              "FR",
              "PT",
              "IT",
              "CN",
              "ES",
              "RU",
              "TR",
              "DE",
              "HI"
            ]
          },
          "wheel_only": {
            "anyOf": [
              {
                "type": "boolean"
              },
              {
                "type": "null"
              }
            ],
            "title": "Wheel Only",
            "description": "If set to True, only the zodiac wheel will be returned. No additional information will be displayed.",
            "default": false
          },
          "active_points": {
            "anyOf": [
              {
                "items": {
                  "anyOf": [
                    {
                      "type": "string",
                      "enum": [
                        "Sun",
                        "Moon",
                        "Mercury",
                        "Venus",
                        "Mars",
                        "Jupiter",
                        "Saturn",
                        "Uranus",
                        "Neptune",
                        "Pluto",
                        "Mean_Node",
                        "True_Node",
                        "Mean_South_Node",
                        "True_South_Node",
                        "Chiron",
                        "Mean_Lilith"
                      ]
                    },
                    {
                      "type": "string",
                      "enum": [
                        "Ascendant",
                        "Medium_Coeli",
                        "Descendant",
                        "Imum_Coeli"
                      ]
                    }
                  ]
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Points",
            "description": "The active points to display in the chart.",
            "default": [
              "Sun",
              "Moon",
              "Mercury",
              "Venus",
              "Mars",
              "Jupiter",
              "Saturn",
              "Uranus",
              "Neptune",
              "Pluto",
              "Mean_Node",
              "Chiron",
              "Ascendant",
              "Medium_Coeli",
              "Mean_Lilith",
              "Mean_South_Node"
            ],
            "examples": [
              [
                "Sun",
                "Moon",
                "Mercury",
                "Venus",
                "Mars",
                "Jupiter",
                "Saturn",
                "Uranus",
                "Neptune",
                "Pluto",
                "Mean_Node",
                "Chiron",
                "Ascendant",
                "Medium_Coeli",
                "Mean_Lilith",
                "Mean_South_Node"
              ]
            ]
          },
          "active_aspects": {
            "anyOf": [
              {
                "items": {
                  "$ref": "#/components/schemas/ActiveAspect"
                },
                "type": "array"
              },
              {
                "type": "null"
              }
            ],
            "title": "Active Aspects",
            "description": "The active aspects to display in the chart.",
            "default": [
              {
                "name": "conjunction",
                "orb": 10
              },
              {
                "name": "opposition",
                "orb": 10
              },
              {
                "name": "trine",
                "orb": 8
              },
              {
                "name": "sextile",
                "orb": 6
              },
              {
                "name": "square",
                "orb": 5
              },
              {
                "name": "quintile",
                "orb": 1
              }
            ],
            "examples": [
              [
                {
                  "name": "conjunction",
                  "orb": 10
                },
                {
                  "name": "opposition",
                  "orb": 10
                },
                {
                  "name": "trine",
                  "orb": 8
                },
                {
                  "name": "sextile",
                  "orb": 6
                },
                {
                  "name": "square",
                  "orb": 5
                },
                {
                  "name": "quintile",
                  "orb": 1
                }
              ]
            ]
          }
        },
        "type": "object",
        "required": [
          "first_subject",
          "transit_subject"
        ],
        "title": "TransitChartRequestModel",
        "description": "The request model for the Transit Chart endpoint."
      },
      "TransitChartResponseModel": {
        "properties": {
          "status": {
            "type": "string",
            "title": "Status",
            "description": "The status of the response."
          },
          "data": {
            "$ref": "#/components/schemas/TransitDataModel",
            "description": "The data of the two subjects."
          },
          "chart": {
            "type": "string",
            "title": "Chart",
            "description": "The SVG chart of the transit."
          },
          "aspects": {
            "items": {
              "$ref": "#/components/schemas/AspectModel"
            },
            "type": "array",
            "title": "Aspects",
            "description": "The aspects between the two subjects."
          }
        },
        "type": "object",
        "required": [
          "status",
          "data",
          "chart",
          "aspects"
        ],
        "title": "TransitChartResponseModel",
        "description": "The response model for the Transit."
      },
      "TransitDataModel": {
        "properties": {
          "first_subject": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel",
            "description": "The data of the first subject."
          },
          "transit": {
            "$ref": "#/components/schemas/AstrologicalSubjectModel",
            "description": "The data of the second subject."
          }
        },
        "type": "object",
        "required": [
          "first_subject",
          "transit"
        ],
        "title": "TransitDataModel",
        "description": "The model for the data of two subjects."
      },
      "TransitSubjectModel": {
        "properties": {
          "year": {
            "type": "integer",
            "title": "Year",
            "description": "The year of birth.",
            "examples": [
              1980
            ]
          },
          "month": {
            "type": "integer",
            "title": "Month",
            "description": "The month of birth.",
            "examples": [
              12
            ]
          },
          "day": {
            "type": "integer",
            "title": "Day",
            "description": "The day of birth.",
            "examples": [
              12
            ]
          },
          "hour": {
            "type": "integer",
            "title": "Hour",
            "description": "The hour of birth.",
            "examples": [
              12
            ]
          },
          "minute": {
            "type": "integer",
            "title": "Minute",
            "description": "The minute of birth.",
            "examples": [
              12
            ]
          },
          "longitude": {
            "anyOf": [
              {
                "type": "number"
              },
              {
                "type": "null"
              }
            ],
            "title": "Longitude",
            "description": "The longitude of the birth location. Defaults on London.",
            "examples": [
              0
            ]
          },
          "latitude": {
            "anyOf": [
              {
                "type": "number"
              },
              {
                "type": "null"
              }
            ],
            "title": "Latitude",
            "description": "The latitude of the birth location. Defaults on London.",
            "examples": [
              51.4825766
            ]
          },
          "city": {
            "type": "string",
            "title": "City",
            "description": "The name of city of birth.",
            "examples": [
              "London"
            ]
          },
          "nation": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "null"
              }
            ],
            "title": "Nation",
            "description": "The name of the nation of birth.",
            "default": "null",
            "examples": [
              "GB"
            ]
          },
          "timezone": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "null"
              }
            ],
            "title": "Timezone",
            "description": "The timezone of the birth location.",
            "examples": [
              "Europe/London"
            ]
          },
          "geonames_username": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "null"
              }
            ],
            "title": "Geonames Username",
            "description": "The username for the Geonames API.",
            "examples": [
              null
            ]
          }
        },
        "type": "object",
        "required": [
          "year",
          "month",
          "day",
          "hour",
          "minute",
          "city"
        ],
        "title": "TransitSubjectModel"
      },
      "ValidationError": {
        "properties": {
          "loc": {
            "items": {
              "anyOf": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            },
            "type": "array",
            "title": "Location"
          },
          "msg": {
            "type": "string",
            "title": "Message"
          },
          "type": {
            "type": "string",
            "title": "Error Type"
          }
        },
        "type": "object",
        "required": [
          "loc",
          "msg",
          "type"
        ],
        "title": "ValidationError"
      }
    },
    "securitySchemes": {
      "RapidAPIKey": {
        "type": "apiKey",
        "name": "x-rapidapi-key",
        "in": "header"
      }
    }
  },
  "servers": [
    {
      "url": "https://astrologer.p.rapidapi.com/"
    }
  ]
}
````

## File: Pipfile
````
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
pydantic = "*"
pydantic-settings = "*"
starlette = "*"
fastapi = "*"
uvicorn = "*"
types-pytz = "*"
pytz = "*"
scour = "*"
typing-extensions = "*"
kerykeion = "*"

[dev-packages]
black = "*"
pytest = "*"
mypy = "*"
httpx = "*"
types-requests = "*"

[requires]
python_version = "3.11"

[scripts]
dev = "uvicorn app.main:app --reload --log-level debug"
test = "pytest -v"
test-verbose = "pytest -vv"
quality = "python -m mypy --ignore-missing-imports ."
schema = "python dump_schema.py"
format = "black . --line-length 200"
````

## File: Procfile
````
web: uvicorn app.main:app --host=0.0.0.0 --port=${PORT:-5000}
````

## File: README.md
````markdown
# Astrologer API

The Astrologer API is a RESTful service providing extensive astrology calculations, designed for seamless integration into projects. It offers a set of astrological charts and data, making it an invaluable tool for both developers and astrology enthusiasts.

Here's an example of a birth chart generated using the Astrologer API:

![John Lenon Chart](https://raw.githubusercontent.com/g-battaglia/kerykeion/refs/heads/master/tests/charts/svg/John%20Lennon%20-%20Dark%20Theme%20-%20Natal%20Chart.svg)


## Quick Endpoints Overview


| Endpoint                          | Method | Description |
|-----------------------------------|--------|-------------|
| `/api/v4/birth-chart`            | POST   | Generates a full birth chart as an SVG string, including planetary positions and aspects. |
| `/api/v4/synastry-chart`         | POST   | Creates a synastry chart comparing two subjects, displaying their interactions and compatibility, along with an SVG representation. |
| `/api/v4/transit-chart`          | POST   | Generates a transit chart for a subject, showing current planetary influences, with an SVG visual representation. |
| `/api/v4/composite-chart`        | POST   | Computes a composite chart for two subjects using the midpoint method, including aspects and an SVG visual representation. |
| `/api/v4/relationship-score`     | POST   | Calculates a compatibility score (0-44) using the Ciro Discepolo method to assess relationship potential. |
| `/api/v4/natal-aspects-data`     | POST   | Provides detailed birth chart data and aspects without the visual chart. |
| `/api/v4/synastry-aspects-data`  | POST   | Returns synastry-related data and aspects between two subjects, without an SVG chart. |
| `/api/v4/transit-aspects-data`   | POST   | Offers transit chart data and aspects for a subject, without an SVG visual representation. |
| `/api/v4/composite-aspects-data` | POST   | Delivers composite chart data and aspects without generating an SVG chart. |
| `/api/v4/birth-data`             | POST   | Returns essential birth chart data without aspects or visual representation. |
| `/api/v4/now`                    | GET    | Retrieves birth chart data for the current UTC time, excluding aspects and the visual chart. |

## Subscription

To access the Astrologer API, subscribe here:

[Subscribe to Astrologer API](https://rapidapi.com/gbattaglia/api/astrologer/pricing)

## Documentation

Explore the comprehensive API documentation:

- [Swagger Documentation](https://www.kerykeion.net/astrologer-api-swagger/): Interactive documentation with detailed information on all endpoints and parameters.

- [Redoc Documentation](https://www.kerykeion.net/astrologer-api-redoc/): A clean, user-friendly documentation interface for easy reference.

- [OpenAPI Specification](https://raw.githubusercontent.com/g-battaglia/Astrologer-API/master/openapi.json): The full OpenAPI specification for the Astrologer API.

## Getting Started

To begin using the Astrologer API, include your API key in the request headers. This key is essential for authenticating your requests and ensuring they are processed correctly.

### Example Request Headers

Ensure your API requests include the following headers:

```javascript
headers: {
    'X-RapidAPI-Host': 'astrologer.p.rapidapi.com',
    'X-RapidAPI-Key': 'YOUR_API_KEY'
    }
```

Replace `YOUR_API_KEY` with your actual API key obtained during registration.


## Features

### Charts

The Astrologer API provides various `*-chart` endpoints with customizable options:

#### Languages

You can specify the `lang` parameter to select the language for your chart. Available options are:

- `EN`: English (default)
- `FR`: French
- `PT`: Portuguese
- `ES`: Spanish
- `TR`: Turkish
- `RU`: Russian
- `IT`: Italian
- `CN`: Chinese
- `DE`: German
- `HI`: Hindi

Example API request:

```json
{
    "subject": {
        "year": 1980,
        "month": 12,
        "day": 12,
        "hour": 12,
        "minute": 12,
        "longitude": 0,
        "latitude": 51.4825766,
        "city": "London",
        "nation": "GB",
        "timezone": "Europe/London",
        "name": "John Doe",
        "zodiac_type": "Tropic"
    },
    "language": "RU"
}
```

#### Themes

Customize the appearance of your charts using the `theme` parameter. Available themes are:

Available themes:

- `light`: Modern soft-colored light theme

![John Lennon Chart Example](https://raw.githubusercontent.com/g-battaglia/kerykeion/refs/heads/master/tests/charts/svg/John%20Lennon%20-%20Light%20Theme%20-%20Natal%20Chart.svg)

- `dark`: Modern dark theme
  
![John Lennon Chart Example](https://raw.githubusercontent.com/g-battaglia/kerykeion/refs/heads/master/tests/charts/svg/John%20Lennon%20-%20Dark%20Theme%20-%20Natal%20Chart.svg)

- `dark-high-contrast`: High-contrast dark theme

![John Lennon Chart Example](https://raw.githubusercontent.com/g-battaglia/kerykeion/refs/heads/master/tests/charts/svg/John%20Lennon%20-%20Dark%20High%20Contrast%20Theme%20-%20Natal%20Chart.svg)

- `classic`: Traditional colorful theme

![Albert Einstein Chart Example](https://raw.githubusercontent.com/g-battaglia/kerykeion/refs/heads/master/tests/charts/svg/Albert%20Einstein%20-%20Natal%20Chart.svg)

Example API request:

```json
{
    "subject": { /* ... */ },
    "theme": "dark"
}
```


### Zodiac Types

You can choose between the Sidereal and Tropical zodiacs using the `zodiac_type` parameter in the `subject` key of most endpoints.

- `tropic`: Tropical zodiac (default)
- `sidereal`: Sidereal zodiac

If you select `sidereal`, you must also specify the `sidereal_mode` parameter, which offers various ayanamsha (zodiacal calculation modes):

- `FAGAN_BRADLEY`
- `LAHIRI` (standard for Vedic astrology)
- `DELUCE`
- `RAMAN`
- `USHASHASHI`
- `KRISHNAMURTI`
- `DJWHAL_KHUL`
- `YUKTESHWAR`
- `JN_BHASIN`
- `BABYL_KUGLER1`
- `BABYL_KUGLER2`
- `BABYL_KUGLER3`
- `BABYL_HUBER`
- `BABYL_ETPSC`
- `ALDEBARAN_15TAU`
- `HIPPARCHOS`
- `SASSANIAN`
- `J2000`
- `J1900`
- `B1950`

The most commonly used ayanamshas are `FAGAN_BRADLEY` and `LAHIRI`.

Example API request:

```json
{
    "subject": {
        "year": 1980,
        "month": 12,
        "day": 12,
        "hour": 12,
        "minute": 12,
        "longitude": 0,
        "latitude": 51.4825766,
        "city": "London",
        "nation": "GB",
        "timezone": "Europe/London",
        "name": "John Doe",
        "zodiac_type": "Sidereal",
        "sidereal_mode": "FAGAN_BRADLEY"
    }
}
```

### House Systems

The `HouseSystem` parameter defines the method used to divide the celestial sphere into twelve houses. Here are the available options:

- **A**: Equal
- **B**: Alcabitius
- **C**: Campanus
- **D**: Equal (MC)
- **F**: Carter poli-equ.
- **H**: Horizon/Azimut
- **I**: Sunshine
- **i**: Sunshine/Alt.
- **K**: Koch
- **L**: Pullen SD
- **M**: Morinus
- **N**: Equal/1=Aries
- **O**: Porphyry
- **P**: Placidus
- **Q**: Pullen SR
- **R**: Regiomontanus
- **S**: Sripati
- **T**: Polich/Page
- **U**: Krusinski-Pisa-Goelzer
- **V**: Equal/Vehlow
- **W**: Equal/Whole Sign
- **X**: Axial rotation system/Meridian houses
- **Y**: APC houses

Usually, the standard house system used is Placidus (P).

Example API request:

```json
{
    "subject": {
        "year": 1980,
        "month": 12,
        "day": 12,
        "hour": 12,
        "minute": 12,
        "longitude": 0,
        "latitude": 51.4825766,
        "city": "London",
        "nation": "GB",
        "timezone": "Europe/London",
        "name": "John Doe",
        "zodiac_type": "Tropic",
        "house_system": "A"
    }
}
```

This allows you to specify the desired house system for calculating and displaying the positions of celestial bodies.

### Perspective Types

The PerspectiveType defines the viewpoint from which the positions of celestial bodies are calculated. Here are the available options:

- "Apparent Geocentric": Earth-centered and shows the apparent positions of celestial bodies as seen from Earth. This is the most commonly used and the default perspective.
- "Heliocentric": Sun-centered.
- "Topocentric": This perspective is based on the observer's specific location on the Earth's surface.
- "True Geocentric": This perspective is also Earth-centered but shows the true positions of celestial bodies without the apparent shifts caused by Earth's atmosphere.
  
Usually, the standard perspective used is "Apparent Geocentric".

Example usage in an API request:

```json
{
    "subject": {
        "year": 1980,
        "month": 12,
        "day": 12,
        "hour": 12,
        "minute": 12,
        "longitude": 0,
        "latitude": 51.4825766,
        "city": "London",
        "nation": "GB",
        "timezone": "Europe/London",
        "name": "John Doe",
        "zodiac_type": "Tropic",
        "perspective": "Heliocentric"
    }
}
```

This allows you to specify the desired perspective for calculating and displaying the positions of celestial bodies.

### Wheel Only Charts

To generate charts that contain only the zodiac wheel without any textual information, you can use the `wheel_only` option in your API call. When this option is set to `True`, only the zodiac wheel will be returned.

Example API request:

```json
{
    "subject": {
        "year": 1980,
        "month": 12,
        "day": 12,
        "hour": 12,
        "minute": 12,
        "longitude": 0,
        "latitude": 51.4825766,
        "city": "London",
        "nation": "GB",
        "timezone": "Europe/London",
        "name": "John Doe",
        "zodiac_type": "Tropic"
    },
    "wheel_only": true
}
```

This can be useful for creating clean and simple visual representations of the zodiac without any additional clutter.

## Timezones

Accurate astrological calculations require the correct timezone. Refer to the following link for a complete list of timezones:

[List of TZ Database Time Zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

#### Active Points and Aspects

For all the Charts endpoints (Birth Chart, Transit Chart), 
Natal Aspects Data and Synastry Aspects Data you can customize which celestial points to include and which aspects to calculate using the `active_points` and `active_aspects` parameters.

Example API request:

```json
{
    "subject": {
        "year": 1980,
        "month": 12,
        "day": 12,
        "hour": 12,
        "minute": 12,
        "longitude": 0,
        "latitude": 51.4825766,
        "city": "London",
        "nation": "GB",
        "timezone": "Europe/London",
        "name": "John Doe",
        "zodiac_type": "Tropic"
    },
    "active_points": [
        "Sun",
        "Moon",
        "Mercury",
        "Venus",
        "Mars",
        "Jupiter",
        "Saturn",
        "Uranus",
        "Neptune",
        "Pluto",
        "Mean_Node",
        "Chiron",
        "Ascendant",
        "Medium_Coeli",
        "Mean_Lilith",
        "Mean_South_Node"
    ],
    "active_aspects": [
        {
            "name": "conjunction",
            "orb": 10
        },
        {
            "name": "opposition",
            "orb": 10
        },
        {
            "name": "trine",
            "orb": 8
        },
        {
            "name": "sextile",
            "orb": 6
        },
        {
            "name": "square",
            "orb": 5
        },
        {
            "name": "quintile",
            "orb": 1
        }
    ]
}
```

These parameters allow you to:
- Specify which celestial points to include in the chart and calculations
- Define which aspects to calculate along with their orbs (the degree of allowable deviation from exact aspect)

## Automatic Coordinates

It is possible to use automatic coordinates if you do not want to implement a different method for calculating latitude, longitude, and timezone.

To do this, you must pass the `geonames_username` parameter inside the `subject` object in every request that contains the `subject` object.

**Logic**

- If `geonames_username` is present, the `longitude`, `latitude`, and `timezone` parameters are automatically ignored.
- If **NOT** present, all three parameters (`longitude`, `latitude`, and `timezone`) must be specified.

**Recommendation**

It is recommended to use actual coordinates directly for greater accuracy.

**Obtaining a Geonames Username**

If you want to calculate coordinates automatically, you need to obtain a `username` for the Geonames Timezone service. The service is free for up to **10,000 requests per day**.
You can obtain a Geonames username by signing up at <a href="http://www.geonames.org/login" target="_blank">Geonames</a>.

**Example**

```json
{
    "subject": {
        "year": 1980,
        "month": 12,
        "day": 12,
        "hour": 12,
        "minute": 12,
        "city": "Jamaica, New York",
        "nation": "US",
        "name": "John Doe",
        "zodiac_type": "Tropic",
        "geonames_username": "YOUR_GEONAMES_USERNAME"
    }
}
```

## Copyright and License

Astrologer API is Free/Libre Open Source Software with an AGPLv3 license. All the terms and conditions of the AGPLv3 license apply to the Astrologer API.
You can review and contribute to the source code via the official repositories:

- [V4 Astrologer API](https://github.com/g-battaglia/v4.astrologer-api)

Astrologer API is developed by Giacomo Battaglia and is based on Kerykeion, a Python library for astrology calculations by the same author. The underlying tools are built on the Swiss Ephemeris.

Since it is an external API service, integrating data and charts retrieved via the API does not impose any licensing restrictions, allowing use in projects with closed source licenses.

## Commercial Use

The Astrologer API can be freely used in both open-source and closed-source commercial applications without restrictions, as it functions as an external service.

For full compliance, we recommend adding this statement in your Terms and Conditions or elsewhere on your site/app:

---
Astrological data and charts on this site are generated using [AstrologerAPI](https://rapidapi.com/gbattaglia/api/astrologer), an open-source third-party service licensed under AGPL v3. Source code:
- [Astrologer API Github](https://github.com/g-battaglia/Astrologer-API)
---

This guarantees full transparency and complete licensing compliance, leaving no room for doubt.


## Contact & Support  

Need help or have feedback? Reach us at:
[kerykeion.astrology@gmail.com](mailto:kerykeion.astrology@gmail.com)
````
